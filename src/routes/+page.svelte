<script>
	import Timer from './Timer.svelte';

	let open = false;
	let seconds = 60;
	function toggle(){
		if (open == true){
			open = false;
			seconds = 60;
		} else {
			open = true;
		}
	}
	const handleTick = () => (seconds -= 1);
</script>

<div>
	<button on:click={toggle}>{open ? 'Stop' : 'Start'}</button>
	{#if open}
		<p>{seconds}</p>
		<Timer callback={handleTick} />
	{/if}
</div>