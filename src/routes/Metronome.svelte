<script>
  import { onDestroy, onMount } from "svelte";
  import { bpm } from "./bpm.svelte";

  let beatsPerBar = 4;
  let currentBeat = 1;
  let isRunning = false;

  let tapTimes = [];

  let audioCtx;
  onMount(() => {
    audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  });

  // Scheduler variables
  let lookahead = 25; // ms: how often to check for notes to schedule
  let scheduleAheadTime = 0.1; // s: how far ahead to schedule
  let nextNoteTime = 0; // s: when the next note is due
  let schedulerId = null; // for setInterval
  let beatLength = 60 / bpm.currentBpm; // seconds per beat

  let beatCounter = 1;

  let uiTimeouts = [];

  function playClick(emphasized = false, time = 0) {
    const osc = audioCtx.createOscillator();
    const envelope = audioCtx.createGain();

    osc.type = "square";
    osc.frequency.value = emphasized ? 1500 : 1000;
    envelope.gain.setValueAtTime(1, time);
    envelope.gain.exponentialRampToValueAtTime(0.001, time + 0.1);

    osc.connect(envelope);
    envelope.connect(audioCtx.destination);
    osc.start(time);
    osc.stop(time + 0.1);
  }

  function scheduleUI(beat, time) {
    const delayMs = Math.max(0, (time - audioCtx.currentTime) * 1000);
    const id = setTimeout(() => {
      if (!isRunning) return;
      currentBeat = beat;
    }, delayMs);
    uiTimeouts.push(id);
  }

  function nextNote() {
    beatLength = 60 / bpm.currentBpm;
    nextNoteTime += beatLength;
    beatCounter = (beatCounter % beatsPerBar) + 1;
  }

  function scheduler() {
    while (nextNoteTime < audioCtx.currentTime + scheduleAheadTime) {
      const beatForThisNote = beatCounter;
      playClick(beatForThisNote === 1, nextNoteTime);
      scheduleUI(beatForThisNote, nextNoteTime);
      nextNote();
    }
  }

  async function startMetronome() {
    if (isRunning) return;
    if (audioCtx.state === "suspended") {
      await audioCtx.resume();
    }
    isRunning = true;
    beatCounter = 1;
    currentBeat = 1;
    nextNoteTime = audioCtx.currentTime;
    schedulerId = setInterval(scheduler, lookahead);
  }

  function stopMetronome() {
    isRunning = false;
    if (schedulerId) clearInterval(schedulerId);
    schedulerId = null;

    uiTimeouts.forEach(clearTimeout);
    uiTimeouts = [];

    currentBeat = 1;
  }

  function toggleMetronome() {
    isRunning ? stopMetronome() : startMetronome();
  }

  function tapTempo() {
    const now = performance.now();
    tapTimes.push(now);
    if (tapTimes.length > 8) tapTimes.shift();

    if (tapTimes.length >= 2) {
      const intervals = [];
      for (let i = 1; i < tapTimes.length; i++) {
        intervals.push(tapTimes[i] - tapTimes[i - 1]);
      }
      const avgInterval =
        intervals.reduce((a, b) => a + b, 0) / intervals.length;
      bpm.currentBpm = Math.min(
        240,
        Math.max(40, Math.round(60000 / avgInterval))
      );
    }
  }

  function adjustBpm(change) {
    bpm.currentBpm = Math.min(240, Math.max(40, bpm.currentBpm + change));
  }

  onDestroy(() => {
    if (schedulerId) clearInterval(schedulerId);
    uiTimeouts.forEach(clearTimeout);
  });
</script>

<label>
  BPM: {bpm.currentBpm}
  <div>
    <button on:click={(e) => adjustBpm(e.shiftKey ? -5 : -1)}>-</button>

    <input type="range" min="40" max="240" bind:value={bpm.currentBpm} />

    <button on:click={(e) => adjustBpm(e.shiftKey ? 5 : 1)}>+</button>
  </div>
</label>

<div class="indicators" style="grid-template-rows:{beatsPerBar}">
  {#each Array(beatsPerBar) as _, i}
    <div class="indicator" class:active={currentBeat === i + 1}>
      {i + 1}
    </div>
  {/each}
</div>

<label>
  Beats per Bar: {beatsPerBar}
  <input type="range" min="1" max="12" bind:value={beatsPerBar} />
</label>

<div>
  <button on:click={toggleMetronome}>
    {isRunning ? "Stop" : "Start"}
  </button>

  <button on:click={tapTempo}> Tap Tempo </button>
</div>

<style>
  div.indicators {
    display: flex;
  }
  div.indicator {
    width: 20px;
    height: 20px;
    border-radius: 100%;
    background: lightblue;
    float: left;
  }
  div.indicator.active {
    background: lightseagreen;
  }
</style>
