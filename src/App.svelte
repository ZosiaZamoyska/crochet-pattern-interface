<script>
  const stitches = [
    { name: 'Chain', key: 'chain' },
    { name: 'Magic Ring', key: 'magic_ring' },
    { name: 'Single Crochet', key: 'single_crochet' },
    { name: 'Half Double Crochet', key: 'half_double_crochet' },
    { name: 'Double Crochet', key: 'double_crochet' },
    { name: 'Treble Crochet', key: 'treble_crochet' }
  ];
  let selectedStitch = stitches[0].key;
</script>

<div class="container">
  <aside class="sidebar">
    <h2>Stitches</h2>
    <ul>
      {#each stitches as stitch}
        <li>
          <button
            class:selected={selectedStitch === stitch.key}
            on:click={() => selectedStitch = stitch.key}
          >
            {stitch.name}
          </button>
        </li>
      {/each}
    </ul>
  </aside>
  <main class="pattern-area">
    <h1>Crochet Pattern Builder</h1>
    <div class="pattern-placeholder">
      <p>Select a stitch and start building your pattern!</p>
    </div>
  </main>
</div>

<style>
  .container {
    display: flex;
    height: 100vh;
  }
  .sidebar {
    width: 220px;
    background: #f8f8f8;
    border-right: 1px solid #eee;
    padding: 1.5rem 1rem;
  }
  .sidebar h2 {
    margin-top: 0;
    font-size: 1.2rem;
    margin-bottom: 1rem;
  }
  .sidebar ul {
    list-style: none;
    padding: 0;
  }
  .sidebar li {
    margin-bottom: 0.5rem;
  }
  .sidebar button {
    width: 100%;
    padding: 0.5rem 1rem;
    border: none;
    background: #fff;
    border-radius: 4px;
    cursor: pointer;
    text-align: left;
    transition: background 0.2s;
  }
  .sidebar button.selected, .sidebar button:hover {
    background: #ffe0c7;
  }
  .pattern-area {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    padding: 2rem;
    background: #fcfcfc;
  }
  .pattern-area h1 {
    margin-top: 0;
    font-size: 2rem;
    margin-bottom: 2rem;
  }
  .pattern-placeholder {
    border: 2px dashed #ccc;
    border-radius: 8px;
    padding: 3rem 5rem;
    color: #aaa;
    font-size: 1.1rem;
    text-align: center;
  }
</style>
