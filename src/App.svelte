<script lang="ts">
	let gridSize = 40
	let viewX = 0;
	let viewY = 0;

	$: cssGridPosition = {
		x: (viewX < 0 ? viewX + (Math.floor(viewX / gridSize) * -gridSize) : viewX) % gridSize,
		y: (viewY < 0 ? viewY + (Math.floor(viewY / gridSize) * -gridSize) : viewY) % gridSize
	} 
	$: console.log(cssGridPosition);

	let dragStart: {x: number, y: number, viewX: number, viewY: number} | null = null;

	function drag(e: MouseEvent) {
		if (dragStart == null) return;

		viewX = dragStart.viewX + e.clientX - dragStart.x;
		viewY = dragStart.viewY + e.clientY - dragStart.y;
	}

	function endDrag() {
		dragStart = null
	}
</script>

<svelte:window 
	on:mousedown={(e) => dragStart = {x: e.clientX, y: e.clientY, viewX: viewX, viewY: viewY}}
	on:mousemove={drag}
	on:mouseup={endDrag}
/>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class='grid' 
	style:--grid-x='{cssGridPosition.x}px' 
	style:--grid-y='{cssGridPosition.y}px' 
>

</div>