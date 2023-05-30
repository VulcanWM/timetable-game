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
		} else {
			ended = false;
			open = true;
			correct = 0;
			incorrect = 0;
			guess = "";
			seconds = 60;
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
	$: speed = Math.floor(60/correct*100)/100
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

	/**
     * @param {{ keyCode: number; }} e
     */
	function onKeyDown(e) {
		if (e.keyCode > 47 && e.keyCode < 58){
			let inputNum = e.keyCode - 48
			guess += inputNum.toString()
		} else if (e.keyCode == 8){
			guess = guess.slice(0, -1)
		} else if (e.keyCode == 13){
			handleSubmit()
		}
	}
</script>

<style>

.time {
	border-radius: 50%;
	padding: 7.5px;
	color: white;
	background-color: blue;
}

.correct {
	padding: 7.5px;
	background-color: green;
	color: white;
}

.incorrect {
	padding: 7.5px;
	background-color: red;
	color: white;
}

.question {
	display: inline;
}

#keypad {
	display: flex;
	flex-wrap: wrap; 
 	align-items: center;
	justify-content: center;
}

#keypad button {
	flex-grow: 1;
	width: 30%;
	padding-top: 20px;
	padding-bottom: 20px;
	margin: 5px;
}

.guess {
	padding: 10px;
	width: 90%;
	margin: 10px;
	color: black;
	background-color: white;
	border: 1px solid black;
	text-align: center;
}
</style>

<div>
	<button on:click={toggle}>{open ? 'End Game' : 'Start'}</button>
	{#if open}
		<br/><br/><span class="time">{seconds}</span>
		<Timer callback={handleTick} />
		<p class="question">What is {num1} x {num2}?</p>
		<span class="correct">{correct}</span>
		<span class="incorrect">{incorrect}</span><br/>
		<input class="guess" bind:value={guess} disabled/>
		<div id="keypad">
			<button on:click={() => guess += "1"}>1</button>
			<button on:click={() => guess += "2"}>2</button>
			<button on:click={() => guess += "3"}>3</button>
			<button on:click={() => guess += "4"}>4</button>
			<button on:click={() => guess += "5"}>5</button>
			<button on:click={() => guess += "6"}>6</button>
			<button on:click={() => guess += "7"}>7</button>
			<button on:click={() => guess += "8"}>8</button>
			<button on:click={() => guess += "9"}>9</button>
			<button on:click={() => guess = guess.slice(0, -1)}>Delete</button>
			<button on:click={() => guess += "0"}>0</button>
			<button on:click={handleSubmit}>Enter</button>
		</div>
	{/if}
	{#if ended}
		<p>The game has ended! You got <span class="correct">{correct}</span> questions correct and <span class="incorrect">{incorrect}</span> questions incorrect!</p>
		<p>You got {percentage}% of questions correct!</p>
	<p>Your speed was {speed} seconds per question.</p>
	{/if}
</div>
<svelte:window on:keydown|preventDefault={onKeyDown} />
