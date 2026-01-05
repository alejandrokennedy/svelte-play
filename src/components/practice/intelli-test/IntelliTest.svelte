<script>
	import Scrolly from "../helpers/Scrolly.svelte";
	import Scatterplot from "./Scatterplot.svelte";

	let value = $state(null);
	const steps = ["<p>Step 1</p>", "<p>Step 2</p>", "<p>Step 3</p>"];
</script>

<section>
	<div class="hero">
		<h1>IntelliTest Scroll</h1>
		<h2>
			By <a href="https://www.linkedin.com/in/alejandrokennedy" target="_blank"
				>Alex Kennedy</a
			>
		</h2>
	</div>
	<div class="section-container">
		<div class="steps-container">
			<Scrolly bind:value class="scrolly-div">
				{#each steps as text, i}
					{$inspect("i", i)}
					<div class="step" class:active={value === i}>
						<div class="step-content">{@html text}</div>
					</div>
				{/each}
				<div class="spacer"></div>
			</Scrolly>
		</div>
		<div class="sticky">
			<Scatterplot step={value} />
		</div>
	</div>
</section>

<style>
	:global(body) {
		overflow-x: hidden;
	}

	.hero {
		display: flex;
		flex-direction: column;
		height: 60vh;
		place-items: center;
		justify-content: center;
		text-align: center;
	}

	.hero h2 {
		margin-top: 0;
		font-weight: 200;
	}

	.spacer {
		height: 40vh;
	}

	.sticky {
		position: sticky;
		top: 10%;
		flex: 1 1 60%;
		width: 60%;
		/*margin: 5px;*/
	}

	.section-container {
		margin-top: 1em;
		text-align: center;
		transition: background 100ms;
		display: flex;
	}

	.step {
		height: 80vh;
		display: flex;
		place-items: center;
		justify-content: center;
	}

	.step-content {
		font-size: 1rem;
		background: whitesmoke;
		color: #ccc;
		border-radius: 5px;
		padding: 0.5rem 1rem;
		display: flex;
		flex-direction: column;
		justify-content: center;
		transition: background 500ms ease;
		box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2);
		text-align: left;
		width: 75%;
		margin: auto;
		max-width: 500px;
	}

	.step.active .step-content {
		background: white;
		color: black;
	}

	.steps-container,
	.sticky {
		height: 100%;
	}

	.steps-container {
		flex: 1 1 40%;
		z-index: 10;
	}

	/* Comment out the following line to always make it 'text-on-top' */
	@media screen and (max-width: 768px) {
		.section-container {
			flex-direction: column-reverse;
		}
		.sticky {
			width: 95%;
			margin: auto;
		}
	}
</style>
