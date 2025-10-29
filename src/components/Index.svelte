<script>
	import { getContext } from "svelte";
	import Footer from "$components/Footer.svelte";
	import Map from "$components/practice/Map.svelte";
	import Circles from "./practice/Circles.svelte";

	// const copy = getContext("copy");
	// const data = getContext("data");
</script>

<svelte:boundary onerror={(e) => console.error(e)}>
	<Circles />
	<Map />
	<!-- <Footer recirc={true} /> -->
</svelte:boundary>
