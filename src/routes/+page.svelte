<script>
	import Timer from './Timer.svelte';

	let open = false;
	let ended = false;
	let seconds = 60;
	let correct = 0;
	let incorrect = 0;
	let guess = ""
	function toggle(){
		if (open == true){
			open = false;
			seconds = 60;
			ended = false;
			guess = "";
			correct = 0;
			incorrect = 0;
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
	$: percentage = Math.floor((correct/(correct+incorrect))*100)
	function handleSubmit() {
		if (parseInt(guess) == answer){
			correct += 1
		} else {
			incorrect += 1
		}
		guess = ""
		num1 = Math.floor(Math.random() * 20) + 1;
		num2 = Math.floor(Math.random() * 20) + 1;
	}
</script>

<div>
	<button on:click={toggle}>{open ? 'End Game' : 'Start'}</button>
	{#if open}
		<p>Correct: {correct}</p>
		<p>Incorrect: {incorrect}</p>
		<p>{seconds}</p>
		<Timer callback={handleTick} />
		<p>What is {num1} x {num2}?</p>
		<form on:submit|preventDefault={handleSubmit}>
			<input bind:value={guess}/>
		</form>
	{/if}
	{#if ended}
		<p>The game has ended! You got {correct} questions correct and {incorrect} questions incorrect!</p>
		<p>You got {percentage}% of questions correct!</p>
	{/if}
</div>