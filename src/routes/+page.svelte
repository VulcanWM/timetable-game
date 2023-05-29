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

<style>

#main {
  background-color: #16a34a;
  position: absolute;
  left: 50%;
  top: 50%;
  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  border: 2px solid black;
  border-radius: 12px;
  padding: 20px;
  width: 50vw;
  text-align: center;
  overflow-wrap: break-word;
  line-height: 1.8;
}

@media only screen and (max-width: 600px) {
  #main {
    width: 60vw;
  }
  p {
    font-size: 12px;
  }
  button {
    font-size: 12px
  }
}

@media only screen and (max-height: 400px) {
  #main {
    width: 95%;
  }
}

@media only screen and (max-width: 400px) {
  #main {
    width: 80vw;
  }
}

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
    flex-basis: 30%;
	padding-top: 20px;
	padding-bottom: 20px;
	margin: 5px;
}
</style>

<div id="main">
	<button on:click={toggle}>{open ? 'End Game' : 'Start'}</button>
	{#if open}
		<br/><br/><span class="time">{seconds}</span>
		<Timer callback={handleTick} />
		<p class="question">What is {num1} x {num2}?</p>
		<span class="correct">{correct}</span>
		<span class="incorrect">{incorrect}</span>
		<form on:submit|preventDefault={handleSubmit}>
			<input bind:value={guess}/>
		</form>
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
	{/if}
</div>