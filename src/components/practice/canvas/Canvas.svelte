<script lang="ts">
	import { onMount } from "svelte";

	let { width, height } = $props();
	let canvas = $state();
	let ctx = $state();

	function scaleCanvas(canvas, ctx, width, height) {
		const devicePixelRatio = window.devicePixelRatio || 1;

		canvas.width = width * devicePixelRatio;
		canvas.height = height * devicePixelRatio;
		canvas.style.width = width + "px";
		canvas.style.height = height + "px";

		ctx.scale(devicePixelRatio, devicePixelRatio);
	}

	onMount(() => {
		ctx = canvas.getContext("2d");
	});

	$effect(() => {
		if (canvas && ctx) {
			console.log("ctx", ctx);
			console.log("width", width);
			console.log("height", height);
			scaleCanvas(canvas, ctx, width, height);
		}
	});

	$inspect("canvas", canvas);
</script>

<canvas bind:this={canvas}></canvas>
