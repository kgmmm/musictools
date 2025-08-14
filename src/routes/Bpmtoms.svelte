<script>
  import { bpm } from "./bpm.svelte";

  let noteType = "straight";

  const noteFractions = [
    { label: "1", factor: 1 },
    { label: "1/2", factor: 0.5 },
    { label: "1/4", factor: 0.25 },
    { label: "1/8", factor: 0.125 },
    { label: "1/16", factor: 0.0625 },
    { label: "1/32", factor: 0.03125 },
  ];

  const msPerBeat = () => (60 / bpm.currentBpm) * 1000;

  const duration = (fraction) => {
    let baseMs = msPerBeat() * (fraction / 0.25);
    if (noteType === "dotted") baseMs *= 1.5;
    if (noteType === "triplet") baseMs *= 2 / 3;
    return baseMs;
  };

  const modifiedLabel = (label) => {
    if (noteType === "dotted") return `${label}<sup>D</sup>`;
    if (noteType === "triplet") return `${label}<sup>T</sup>`;
    return label;
  };
</script>

<div>
  <label>
    <input type="radio" bind:group={noteType} value="straight" /> Straight
  </label>
  <label>
    <input type="radio" bind:group={noteType} value="dotted" /> Dotted
  </label>
  <label>
    <input type="radio" bind:group={noteType} value="triplet" /> Triplet
  </label>
</div>

<table>
  <thead>
    <tr>
      {#each noteFractions as nf}
        <th>{@html modifiedLabel(nf.label)}</th>
      {/each}
    </tr>
  </thead>
  <tbody>
    <tr>
      {#each noteFractions as nf}
        <td>{duration(nf.factor).toFixed(2)} ms</td>
      {/each}
    </tr>
  </tbody>
</table>

<style>
  table {
    text-align: center;
    width: 100%;
  }
  th,
  td {
    box-sizing: border-box;
    width: calc(100% / 6);
    height: 24px;
  }
</style>
