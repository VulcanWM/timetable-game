<script>
	import Timer from './Timer.svelte';

	let open = false;
	let ended = false;
	let seconds = 60;
	let score = 0;
	function toggle(){
		if (open == true){
			open = false;
			seconds = 60;
		} else {
			open = true;
		}
	}
	function handleTick(){
		seconds -= 1
		if (seconds == 0){
			toggle();
			ended = true;
		}
	}
	let num1 = Math.floor(Math.random() * 20) + 1;
	let num2 = Math.floor(Math.random() * 20) + 1;
	$: answer = num1 * num2
</script>

<div>
	<button on:click={toggle}>{open ? 'Stop' : 'Start'}</button>
	{#if open}
		<p>{seconds}</p>
		<Timer callback={handleTick} />
		<p>What is {num1} x {num2}?</p>
		<input/>
	{/if}
	{#if ended}
		<p>The game has ended! Your score is {score}</p>
	{/if}
</div>