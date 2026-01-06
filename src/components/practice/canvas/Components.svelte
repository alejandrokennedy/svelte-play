<script lang="ts">
	import { scaleLinear } from "d3";
	import Circle from "./Circle.svelte";
	import Canvas from "./Canvas.svelte";
	import CanvasCircle from "./CanvasCircle.svelte";

	let data = $state([]);

	setInterval(() => {
		data = Array.from({ length: 1000 }).map(() => {
			return {
				a: Math.random(),
				b: Math.random(),
				r: Math.random(),
				fill: `rgb(${Math.random() * 255}, ${Math.random() * 255}, ${Math.random() * 255})`
			};
		});
	}, 2500);

	let width = $state(1000);
	let height = $state(500);

	let xScale = $derived(scaleLinear().domain([0, 1]).range([0, width]));
	let yScale = $derived(scaleLinear().domain([0, 1]).range([height, 0]));
	let rScale = $derived(
		scaleLinear()
			.domain([0, 1])
			.range([5, width / 100])
	);
</script>

<div id="viz-container" bind:clientWidth={width} bind:clientHeight={height}>
	<Canvas {width} {height}>
		{#each data as { a, b, r, fill }}
			<CanvasCircle x={xScale(a)} y={yScale(b)} r={rScale(r)} {fill} />
		{/each}
	</Canvas>
	<svg id="svg" {width} {height}>
		{#each data as { a, b, r, fill }}
			<Circle x={xScale(a)} y={yScale(b)} r={rScale(r)} {fill} />
		{/each}
	</svg>
</div>

<style>
	#viz-container {
		width: 100vw;
		height: 100vh;
	}
</style>
