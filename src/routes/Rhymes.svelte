<script>
  let inputWord = "";
  let rhymes = [];
  let loading = false;
  let error = "";
  let hasSearched = false;
  let lastSearched = "";

  async function fetchRhymes() {
    if (!inputWord.trim()) return;
    if (inputWord == lastSearched) return;

    loading = true;
    error = "";
    rhymes = [];
    hasSearched = false;

    const url = `https://rhymebrain.com/talk?function=getRhymes&word=${encodeURIComponent(
      inputWord
    )}&maxResults=50`;

    try {
      const res = await fetch(url);
      if (!res.ok) throw new Error(`API error: ${res.statusText}`);
      const data = await res.json();
      rhymes = data;
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
      hasSearched = true;
      lastSearched = inputWord;
    }
  }
</script>

<div class="controls">
  <input
    bind:value={inputWord}
    placeholder="Enter a word..."
    on:keydown={(e) => e.key === "Enter" && fetchRhymes()}
  />

  <button on:click={fetchRhymes} disabled={loading}>
    {loading ? "Loading..." : "Find Rhymes"}
  </button>
</div>

<div class="output">
  {#if error}
    <p style="color: red;">Error: {error}</p>
  {:else if rhymes.length}
    <ul>
      {#each rhymes as rhyme}
        <li>{rhyme.word} (score: {rhyme.score})</li>
      {/each}
    </ul>
  {:else if hasSearched && !loading && rhymes.length === 0}
    <p>No rhymes found.</p>
  {/if}
</div>

<style>
  input {
    width: 128px;
  }
  div.output {
    margin-top: 16px;
  }
  div.controls {
    width: 100%;
    background: black;
    position: sticky;
    top: 0;
    left: 0;
  }
</style>
