<script>
	import { getContext } from "svelte";
	// import Footer from "$components/Footer.svelte";
	import Map from "$components/practice/Map.svelte";
	import Circles from "./practice/Circles.svelte";
	import PlasticsTreaty from "./plastics-treaty/PlasticsTreaty.svelte";
	import Demo from "./demo/Demo.svelte";

	// const copy = getContext("copy");
	// const data = getContext("data");
</script>

<svelte:boundary onerror={(e) => console.error(e)}>
	<PlasticsTreaty />
	<Map /> -->
	<Demo />
	<!-- <Circles /> -->
	<!-- <Footer recirc={true} /> -->
</svelte:boundary>
