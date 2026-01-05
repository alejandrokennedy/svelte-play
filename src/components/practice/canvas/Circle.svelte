<script lang="ts">
	let { x, y, r, fill } = $props();
</script>

<circle class="circle" cx={x} cy={y} {r} {fill} />

<style>
	.circle {
		transition: 2s;
		opacity: 0.8;
	}
</style>
