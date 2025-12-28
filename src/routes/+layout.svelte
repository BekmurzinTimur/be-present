<script lang="ts">
	import './layout.css';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={'/assets/icon48.png'} />
	<title>Be Present</title>
	<meta name="description" content="Gentle reminders to stay present" />
</svelte:head>
{@render children()}
