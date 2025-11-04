<script lang="ts">
	import { json, geoAlbersUsa, geoPath } from "d3";
	import { onMount } from "svelte";
	import type { FeatureCollection } from "geojson";

	let width = $state(600);
	let height = $state(400);
	let geojson = $state<FeatureCollection | null>(null);
	let projection = $derived(geoAlbersUsa().fitSize([width, height], geojson));
	let pathGenerator = $derived(geoPath(projection));
	let counties = $derived(
		geojson?.features.map((feature) => {
			return { ...feature, path: pathGenerator(feature) };
		}) ?? []
	);
	let hoveredCountyId = $state(null);
	// $inspect("hoveredCountyId", hoveredCountyId);
	$inspect("counties", counties);

	let loading = $state(true);
	let error = $state<string | null>(null);

	const geojsonPath =
		"https://raw.githubusercontent.com/plotly/datasets/master/geojson-counties-fips.json";

	onMount(() => {
		const controller = new AbortController();

		const fetchData = async (url: string) => {
			loading = true;
			error = null;

			try {
				const result = (await json(url, {
					signal: controller.signal
				})) as FeatureCollection;
				geojson = result;
			} catch (err) {
				if (err instanceof Error && err.name === "AbortError") {
					return; //Component unmounted
				}
				error = err instanceof Error ? err.message : "Failed to fetch data";
				console.error(`error fetching data: ${err}`);
			} finally {
				loading = false;
			}
		};

		fetchData(geojsonPath);

		return () => {
			controller.abort();
		};
	});
</script>

<main bind:clientWidth={width} bind:clientHeight={height}>
	{#if loading}
		<p>loading...</p>
	{:else if error}
		<p>Error: {error}</p>
	{:else}
		<p>loaded {geojson?.features.length ?? 0} features</p>
		<svg {width} {height}>
			{#each counties as { id, path }}
				<path
					d={path}
					onmouseenter={() => (hoveredCountyId = id)}
					class={id === hoveredCountyId ? "active" : ""}
					role="none"
				></path>
			{/each}
		</svg>
	{/if}
</main>

<style>
	main {
		width: 100vw;
		height: 100vh;
		overflow: hidden;
	}

	path {
		stroke: none;
		/*stroke: white;
    stroke-width: 0.5;*/
		fill: purple;
		opacity: 0.5;
		transition: opacity 0.4s ease-in-out;
		transition-delay: 0.2s;
	}

	path.active {
		opacity: 1;
		stroke: white;
		transition-duration: 0s;
		transition-delay: 0s;
	}
</style>
