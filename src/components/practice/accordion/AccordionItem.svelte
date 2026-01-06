<script lang="ts">
	// let { children, title } = $props();
	let { accordionItem } = $props();

	let open = $state(false);

	function toggle() {
		open = !open;
	}
</script>

<div class="accordion-item">
	{@render accordionItem?.({ open, toggle })}
</div>
