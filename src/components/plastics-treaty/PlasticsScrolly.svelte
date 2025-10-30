<script>
	import Scrolly from "$components/helpers/Scrolly.svelte";
	import worldCountries from "$data/worldCountries.json";
	console.log("worldCountries", worldCountries);

	let value = $state(null);
	let width = $state(600);
	let height = $state(400);
</script>

<section id="plastics-scrolly">
	<div id="viz-container" bind:clientWidth={width} bind:clientHeight={height}>
		viz: {width} x {height}
	</div>
	<div class="spacer"></div>
	<Scrolly bind:value>
		{#each [0, 1, 2, 3, 4] as text, i}
			<div class="step">
				<p>{text}</p>
			</div>
		{/each}
	</Scrolly>
</section>

<style>
	#plastics-scrolly {
		/*background-color: bisque;*/
	}

	#viz-container {
		background-color: aliceblue;
		opacity: 0.65;
		position: sticky;
		top: 0em;
		border: 1px solid orangered;
		/*top: 4em;*/
		height: 100vh;
		/*height: 50vh;*/
	}

	.step {
		height: 80vh;
		background: var(--color-gray-100);
		text-align: center;
	}

	.step p {
		padding: 1rem;
	}
</style>
