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

<h1>musictools</h1>

<Notetohz />
<Metronome />
<Bpmtoms />
<Rhymes />
