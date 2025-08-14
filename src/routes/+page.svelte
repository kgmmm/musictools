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
    <Notetohz />
  </section>
  <section class="metronome">
    <Metronome />
  </section>
  <section class="rhymes">
    <Rhymes />
  </section>
  <section class="bpmtoms">
    <Bpmtoms />
  </section>
</article>

<style>
  article.wrapper {
    margin: 0 auto;
    padding: 16px 0px;
    box-sizing: border-box;
    width: 1152px;
    display: grid;
    grid-template-columns: repeat(3, 384px);
  }

  section.notetohz,
  section.metronome,
  section.rhymes {
    width: 384px;
    height: 291px;
  }
  section.rhymes {
    overflow-y: scroll;
    position: relative;
  }
  section.bpmtoms {
    width: calc(384 * 3px);
    grid-column: 1 / 3;
  }
</style>
