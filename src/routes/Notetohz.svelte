<script>
  const A4_FREQ = 440.0;
  const A4_MIDI = 69;
  const NOTE_NAMES = [
    "C",
    "C♯/D♭",
    "D",
    "D♯/E♭",
    "E",
    "F",
    "F♯/G♭",
    "G",
    "G♯/A♭",
    "A",
    "A♯/B♭",
    "B",
  ];

  function midiToFreq(midi) {
    return A4_FREQ * Math.pow(2, (midi - A4_MIDI) / 12);
  }

  function generateNoteTable(startMidi = 24, endMidi = 120) {
    const table = [];

    for (let midi = startMidi; midi <= endMidi; midi++) {
      const noteName = NOTE_NAMES[midi % 12];
      const octave = Math.floor(midi / 12) - 1;
      const fullNote = `${noteName}${octave}`;
      const freq = midiToFreq(midi).toFixed(2);

      table.push({ note: fullNote, frequency: freq });
    }

    return table;
  }

  const notes = generateNoteTable();

  const uniqueNoteNames = [
    ...Array.from(new Set(notes.map((n) => n.note.replace(/\d+$/, "")))),
  ];

  let selectedNote = $state("C");

  let filteredNotes = $derived(
    notes.filter((n) => n.note.replace(/\d+$/, "") === selectedNote)
  );
</script>

<label for="note">
  Filter by Note:&nbsp;
  <select bind:value={selectedNote} id="note">
    {#each uniqueNoteNames as name}
      <option value={name}>{name}</option>
    {/each}
  </select>
</label>

<table>
  <thead>
    <tr>
      <th>Note</th>
      <th>Frequency (Hz)</th>
    </tr>
  </thead>
  <tbody>
    {#each filteredNotes as { note, frequency }}
      <tr>
        <td>{note}</td>
        <td>{frequency}</td>
      </tr>
    {/each}
  </tbody>
</table>

<style>
  table {
    text-align: left;
  }
  th,
  td {
    box-sizing: border-box;
    min-width: 128px;
    height: 24px;
  }
</style>
