<script lang="ts">
	import './layout.css';

	let { children } = $props();
</script>

<svelte:head><link rel="icon" href={'/assets/icon48.png'} /></svelte:head>
{@render children()}
