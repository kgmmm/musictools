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

<label>
  Filter by Note:&nbsp;
  <select bind:value={selectedNote}>
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
    border-collapse: collapse;
    margin-top: 1rem;
  }

  th,
  td {
    padding: 8px 12px;
    border: 1px solid #ddd;
  }
  td:hover {
    background: rgba(255, 255, 255, 0.15);
  }
  tr:hover {
    background: rgba(255, 255, 255, 0.1);
  }

  th {
    color: #555;
    background-color: #f4f4f4;
  }
</style>
