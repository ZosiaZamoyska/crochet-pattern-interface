<script>
  export let mode = 'grid'; // 'grid' or 'round'
  export let stitches = [];
  export let selectedStitchType = 'chain';
  export let onAddStitch = () => {};

  // Grid settings
  const gridSize = 50;
  const gridRows = 10;
  const gridCols = 14;

  // Round settings
  const centerX = 400;
  const centerY = 300;
  const roundRadiusStep = 50;
  const stitchesPerRound = 12;
  const maxRounds = 6;

  let isDragging = false;
  let lastSnapped = null;

  function getSnappedPoint(x, y) {
    if (mode === 'grid') {
      x = Math.round(x / gridSize) * gridSize;
      y = Math.round(y / gridSize) * gridSize;
      return { x, y };
    } else {
      let dx = x - centerX;
      let dy = y - centerY;
      let r = Math.sqrt(dx*dx + dy*dy);
      let round = Math.round(r / roundRadiusStep);
      let angle = Math.atan2(dy, dx);
      if (angle < 0) angle += 2 * Math.PI;
      let posInRound = Math.round(angle / (2 * Math.PI / stitchesPerRound));
      let snappedAngle = posInRound * (2 * Math.PI / stitchesPerRound);
      let snappedR = round * roundRadiusStep;
      x = centerX + Math.cos(snappedAngle) * snappedR;
      y = centerY + Math.sin(snappedAngle) * snappedR;
      return { x, y, round, posInRound };
    }
  }

  function handleCanvasClick(event) {
    // Only add on click if not dragging
    if (isDragging) return;
    const rect = event.target.getBoundingClientRect();
    let x = event.clientX - rect.left;
    let y = event.clientY - rect.top;
    let snapped = getSnappedPoint(x, y);
    onAddStitch({
      type: selectedStitchType,
      ...snapped
    });
  }

  function handlePointerDown(event) {
    isDragging = true;
    lastSnapped = null;
    handlePointerMove(event); // Add first point
    window.addEventListener('pointermove', handlePointerMove);
    window.addEventListener('pointerup', handlePointerUp);
  }

  function handlePointerMove(event) {
    const svg = event.target.closest('svg');
    const rect = svg.getBoundingClientRect();
    let x = event.clientX - rect.left;
    let y = event.clientY - rect.top;
    let snapped = getSnappedPoint(x, y);
    // Only add if not same as last
    if (!lastSnapped || snapped.x !== lastSnapped.x || snapped.y !== lastSnapped.y) {
      onAddStitch({
        type: selectedStitchType,
        ...snapped
      });
      lastSnapped = snapped;
    }
  }

  function handlePointerUp() {
    isDragging = false;
    lastSnapped = null;
    window.removeEventListener('pointermove', handlePointerMove);
    window.removeEventListener('pointerup', handlePointerUp);
  }

  // SVG symbol paths for a few stitches
  const stitchSymbols = {
    chain: 'M0,0 Q5,10 10,0',
    single_crochet: 'M0,0 L0,15 M-5,7.5 L5,7.5',
    double_crochet: 'M0,0 L0,20 M-5,10 L5,10 M-5,15 L5,15',
    magic_ring: 'M0,0 m-8,0 a8,8 0 1,0 16,0 a8,8 0 1,0 -16,0',
    half_double_crochet: 'M0,0 L0,18 M-5,9 L5,9',
    treble_crochet: 'M0,0 L0,25 M-5,8 L5,8 M-5,16 L5,16 M-5,20 L5,20'
  };
</script>

<svg width="800" height="600" style="background:#fff; border:1px solid #ccc;" 
  on:click={handleCanvasClick}
  on:pointerdown={handlePointerDown}
>
  {#if mode === 'grid'}
    <!-- Draw grid -->
    {#each Array.from({length: gridCols}) as _, colIdx (colIdx)}
      <line x1={colIdx*gridSize} y1={0} x2={colIdx*gridSize} y2={gridRows*gridSize} stroke="#eee" />
    {/each}
    {#each Array.from({length: gridRows}) as _, rowIdx (rowIdx)}
      <line x1={0} y1={rowIdx*gridSize} x2={gridCols*gridSize} y2={rowIdx*gridSize} stroke="#eee" />
    {/each}
  {:else}
    <!-- Draw rounds -->
    {#each Array.from({length: maxRounds}) as _, rIdx (rIdx)}
      <circle cx={centerX} cy={centerY} r={(rIdx+1)*roundRadiusStep} fill="none" stroke="#eee" />
    {/each}
    <!-- Draw spokes -->
    {#each Array.from({length: stitchesPerRound}) as _, iIdx (iIdx)}
      <line
        x1={centerX}
        y1={centerY}
        x2={centerX + Math.cos(iIdx * 2 * Math.PI / stitchesPerRound) * maxRounds * roundRadiusStep}
        y2={centerY + Math.sin(iIdx * 2 * Math.PI / stitchesPerRound) * maxRounds * roundRadiusStep}
        stroke="#eee"
      />
    {/each}
  {/if}

  <!-- Draw stitches and connections -->
  {#each stitches as stitch, i (stitch.id)}
    {#if i > 0}
      <!-- Connect to previous stitch -->
      <line
        x1={stitches[i-1].x}
        y1={stitches[i-1].y}
        x2={stitch.x}
        y2={stitch.y}
        stroke="#aaa"
        stroke-width="2"
      />
    {/if}
    <!-- Draw the stitch symbol -->
    <g transform={`translate(${stitch.x},${stitch.y})`}>
      <path d={stitchSymbols[stitch.type] || stitchSymbols.chain} stroke="#333" stroke-width="2" fill="none" />
      <circle r="3" fill="#ffb347" />
    </g>
  {/each}
</svg>

<style>
svg {
  cursor: crosshair;
}
</style> 