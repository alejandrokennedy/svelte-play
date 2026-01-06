<script lang="ts">
	import { Accordion, AccordionItem } from "./";
</script>

<Accordion>
	<AccordionItem>
		{#snippet accordionItem({ open, toggle })}
			<button class="accordion-heading">
				<div>Item A</div>
				<div onclick={toggle} class="accordion-trigger" class:open>👈</div>
			</button>

			{#if open}
				<div class="accordion-content">content of A</div>
			{/if}
		{/snippet}
	</AccordionItem>
</Accordion>
