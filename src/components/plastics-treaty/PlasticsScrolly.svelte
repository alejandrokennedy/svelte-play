<script lang="ts">
	import Scrolly from "$components/helpers/Scrolly.svelte";
	import worldCountries from "$data/worldCountries.json";
	import { geoEqualEarth, geoPath } from "d3";
	import { feature, mesh } from "topojson-client";

	let value = $state(null);
	let width = $state(600);
	let height = $state(400);
	let geojson = $state(
		feature(worldCountries, worldCountries.objects.countries)
	);
	let projection = $derived(geoEqualEarth().fitSize([width, height], geojson));
	let pathGenerator = $derived(geoPath(projection));
	let countries = $derived(
		geojson?.features.map((country) => {
			return { ...country, path: pathGenerator(country) };
		}) ?? []
	);

	$inspect("geojson", geojson);
	$inspect("geojson?.features", geojson?.features);
</script>

<section id="plastics-scrolly">
	<div id="viz-container" bind:clientWidth={width} bind:clientHeight={height}>
		<!-- viz: {width} x {height} ----------- active: {value} -->
		<svg id="svg" {width} {height}>
			<g id="country-group">
				{#each countries as { path }}
					<path d={path}></path>
				{/each}
			</g>
		</svg>
	</div>
	<div class="spacer"></div>
	<Scrolly bind:value>
		{#each [0, 1, 2, 3, 4] as text, i}
			{@const active = value === i}
			<div class="step" class:active>
				<p>{text}</p>
			</div>
		{/each}
	</Scrolly>
</section>

<style>
	#country-group {
		fill: whitesmoke;
		stroke: dimgrey;
	}

	#viz-container {
		/*opacity: 0.6;*/
		position: sticky;
		top: 0em;
		border: 2px solid orangered;
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
