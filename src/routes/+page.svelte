<script>
  import Notetohz from "./Notetohz.svelte";
  import Rhymes from "./Rhymes.svelte";
  import Bpmtoms from "./Bpmtoms.svelte";
  import Metronome from "./Metronome.svelte";
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://fixedsys.javanile.org/css/fixedsys-dark.css"
  />
</svelte:head>

<article class="wrapper">
  <section class="notetohz">
    <fieldset>
      <legend>Covert note to HZ: </legend>
      <Notetohz />
    </fieldset>
  </section>
  <section class="metronome">
    <fieldset>
      <legend>Metronome: </legend>
      <Metronome />
    </fieldset>
  </section>
  <section class="rhymes">
    <fieldset>
      <legend>Rhymebrain: </legend>
      <Rhymes />
    </fieldset>
  </section>
  <section class="bpmtoms">
    <fieldset>
      <legend>BPM subdivisions: </legend>
      <Bpmtoms />
    </fieldset>
  </section>
  <section class="links">
    <fieldset>
      <legend>External links: </legend>
      <ul>
        <li>
          <a href="https://www.hooktheory.com/trends" target="_blank"
            >hooktheory</a
          >
        </li>
        <li>
          <a href="https://www.scales-chords.com/chordid.php" target="_blank"
            >chordtools</a
          >
        </li>
      </ul>
    </fieldset>
  </section>
</article>

<style>
  fieldset {
    box-sizing: border-box;
    padding: 0px 16px;
    padding-bottom: 16px;
    border: 1px solid white;
    border-radius: 3px;
    margin-bottom: 0;
    height: 100%;
  }
  legend {
    width: unset;
    margin-bottom: 8px;
  }

  article.wrapper {
    margin: 0 16px;
    padding: 16px 0px;
    box-sizing: border-box;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    row-gap: 16px;
    column-gap: 16px;
  }

  section.notetohz,
  section.metronome,
  section.rhymes,
  section.links,
  section.bpmtoms {
    align-self: stretch;
  }
  section.bpmtoms {
    grid-column: 1 / 3;
  }
  section.links {
    grid-column: 3 / 3;
    grid-row: 2 / 2;
  }

  ul {
    margin-top: 16px;
  }
</style>
