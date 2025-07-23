<script>
  import PatternCanvas from './lib/PatternCanvas.svelte';
  const stitches = [
    { name: 'Chain', key: 'chain' },
    { name: 'Magic Ring', key: 'magic_ring' },
    { name: 'Single Crochet', key: 'single_crochet' },
    { name: 'Half Double Crochet', key: 'half_double_crochet' },
    { name: 'Double Crochet', key: 'double_crochet' },
    { name: 'Treble Crochet', key: 'treble_crochet' }
  ];
  let selectedStitch = stitches[0].key;
  let mode = 'grid'; // 'grid' or 'round'
  let patternStitches = [];
  let stitchIdCounter = 0;

  function handleAddStitch(stitch) {
    patternStitches = [
      ...patternStitches,
      { ...stitch, id: stitchIdCounter++ }
    ];
  }

  function handleReset() {
    patternStitches = [];
    stitchIdCounter = 0;
  }

  function handleUndo() {
    patternStitches = patternStitches.slice(0, -1);
  }
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
    <div style="margin-top:2rem;">
      <label><input type="radio" bind:group={mode} value="grid"> Rows (Grid)</label><br>
      <label><input type="radio" bind:group={mode} value="round"> Rounds (Circle)</label>
    </div>
  </aside>
  <main class="pattern-area">
    <h1>Crochet Pattern Builder</h1>
    <div class="canvas-controls">
      <button on:click={handleUndo} disabled={patternStitches.length === 0}>Undo</button>
      <button on:click={handleReset} disabled={patternStitches.length === 0}>Reset</button>
    </div>
    <PatternCanvas
      {mode}
      stitches={patternStitches}
      selectedStitchType={selectedStitch}
      onAddStitch={handleAddStitch}
    />
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
  .canvas-controls {
    margin-bottom: 1rem;
    display: flex;
    gap: 1rem;
  }
  .canvas-controls button {
    padding: 0.5rem 1.5rem;
    font-size: 1rem;
    border-radius: 4px;
    border: 1px solid #ccc;
    background: #fff;
    cursor: pointer;
    transition: background 0.2s;
  }
  .canvas-controls button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  .canvas-controls button:not(:disabled):hover {
    background: #ffe0c7;
  }
</style>
