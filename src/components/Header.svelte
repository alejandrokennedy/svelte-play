<script>
	import cenLogo from "$svg/logo-cen-tagline-red.svg";
	import play from "$svg/play.svg";
	import arrowFooter from "$svg/arrow-footer.svg";
	import arrowUpRight from "$svg/arrow-up-right.svg";
</script>

<header>
	<div class="cenLogo">
		<a href="https://cen.acs.org" aria-label="C&EN" target="_self"
			>{@html cenLogo}</a
		>
	</div>
	<!-- <a href="https://cen.acs.org" aria-label="C&EN" target="_self">{@html play}</a
	>
	<a href="https://cen.acs.org" aria-label="C&EN" target="_self"
		>{@html arrowFooter}</a
	>
	<a href="https://cen.acs.org" aria-label="C&EN" target="_self"
		>{@html arrowUpRight}</a
	> -->
</header>

<style>
	.cenLogo {
		max-width: 14em;
		margin: 0 auto;
		padding: 32px 0;
		transform: rotate(-4deg);
	}

	.cenLogo a {
		border: none;
		display: block;
		color: var(--color-fg);
	}

	.cenLogo a:hover {
		background-color: transparent;
	}
</style>
