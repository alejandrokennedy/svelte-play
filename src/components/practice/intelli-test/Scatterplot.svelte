<script lang="ts">
	import { onMount } from "svelte";
	import * as d3 from "d3";

	let { step } = $props();

	$inspect("step", step);

	let data = $state([]);
	let width = $state(400);
	let height = 600;
	let xProperty = $state("weight");
	let yProperty = $state("raceTime");
	let colorProperty = $state("name");

	const margin = {
		top: 30,
		right: 20,
		bottom: 50,
		left: 50
	};

	const x = $derived(
		d3
			.scaleLinear()
			.domain(d3.extent(data, (d) => d[xProperty]))
			.range([margin.left, width - margin.right])
	);

	const y = $derived(
		d3
			.scaleLinear()
			.domain(d3.extent(data, (d) => d[yProperty]))
			.range([height - margin.bottom, margin.top])
	);

	const color = $derived(
		d3
			.scaleOrdinal()
			.domain(data.map((d) => d[colorProperty]))
			.range(d3.schemeCategory10)
	);

	let gx;
	let gy;

	onMount(async () => {
		data = await d3.csv(
			"https://raw.githubusercontent.com/gckirchoff/league-of-pigs-data/refs/heads/master/data.csv",
			(row) => ({
				...row,
				season: +row.season,
				round: +row.round,
				race: +row.race,
				length: +row.length,
				height: +row.height,
				waist: +row.waist,
				weight: +row.weight,
				isMale: row["is male"] === "TRUE",
				position: +row.position,
				raceTime: +row["race time"]
			})
		);
	});

	$effect(() => {
		d3.select(gx).call(d3.axisBottom(x));
		d3.select(gy).call(d3.axisLeft(y));

		if (step === 0) {
			xProperty = "height";
		} else if (step === 1) {
			xProperty = "weight";
		} else if (step === 2) {
			xProperty = "waist";
		}
	});

	$inspect("data", data);
	$inspect("color.domain()", color.domain());
</script>

<div>
	<label>
		X:
		<select bind:value={xProperty}>
			<option value="weight">Weight</option>
			<option value="length">Length</option>
			<option value="height">Height</option>
			<option value="waist">Waist</option>
			<option value="position">Position</option>
		</select>
	</label>
</div>

<div>
	<label>
		Y:
		<select bind:value={yProperty}>
			<option value="raceTime">Race Time</option>
			<option value="position">Position</option>
		</select>
	</label>
</div>

<div bind:clientWidth={width}>
	<svg {width} {height}>
		<g style="transform: translate({margin.left}px, {margin.top}px)">
			<rect
				fill="aliceblue"
				width={width - margin.left - margin.right}
				height={height - margin.top - margin.bottom}
			></rect>
		</g>
		<g
			bind:this={gx}
			transform="translate(0,{height - margin.bottom})"
			color="gray"
		>
			<text
				fill="currentColor"
				font-size={14}
				text-anchor="end"
				transform="translate({width}, {margin.bottom - 4})">{xProperty} →</text
			>
		</g>
		<g bind:this={gy} transform="translate({margin.left},0)" color="gray">
			<text
				fill="currentColor"
				x={-margin.left}
				y={margin.top / 2}
				text-anchor="start"
				font-size={14}>↑ {yProperty}</text
			>
		</g>
		{#each data as d}
			<circle
				cx={x(d[xProperty])}
				cy={y(d[yProperty])}
				fill={color(d[colorProperty])}
				opacity="0.6"
				r="5px"
			/>
		{/each}
	</svg>
</div>

<style>
	svg {
		background-color: white;
	}

	circle {
		transition: all 500ms ease;
	}
</style>
