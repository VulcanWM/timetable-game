<script>
	import Timer from './Timer.svelte';

	let open = true;
	let seconds = 60;

	const toggle = () => (open = !open);
	const handleTick = () => (seconds -= 1);
</script>

<div>
	<button on:click={toggle}>{open ? 'Pause' : 'Start'} Timer</button>
	<p>
		{seconds}
	</p>
	{#if open}
		<Timer callback={handleTick} />
	{/if}
</div>