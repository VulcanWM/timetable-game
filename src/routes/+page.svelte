<script>
	import Timer from "./Timer.svelte";
	let open = false;
	let ended = false;
	let seconds = 60;
	let correct = 0;
	let incorrect = 0;
	let guess = "";
	let xValues = [0];
	let yValues = [0];
	let coords = "";
	const all_timetables = [
		1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20,
	];
	let timetables = [
		1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20,
	];
	let data = [{ time: 0, value: 0 }];
	function toggleStart() {
		if (open == true) {
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
	function handleTick() {
		data.push({ time: 60 - seconds, value: correct });
		data = data;
		xValues.push(60 - seconds);
		xValues = xValues;
		yValues.push(correct);
		yValues = yValues;
		seconds -= 1;
		if (seconds == 0) {
			toggleStart();
			ended = true;
		}
		console.log(xValues.length);
		console.log(yValues.length);
	}
	$: num1 = timetables[Math.floor(Math.random() * timetables.length)];
	$: num2 = Math.floor(Math.random() * 12) + 1;
	$: answer = num1 * num2;
	$: percentage = Math.floor((correct / (correct + incorrect)) * 100);
	$: speed = Math.floor((60 / correct) * 100) / 100;
	function handleSubmit() {
		if (ended == false) {
			if (parseInt(guess) == answer) {
				correct += 1;
			} else {
				incorrect += 1;
			}
			guess = "";
			num1 = timetables[Math.floor(Math.random() * timetables.length)];
			num2 = Math.floor(Math.random() * 12) + 1;
		}
	}
	function createCoords() {
		for (let i = 0; i < xValues.length; i++) {
			coords =
				coords +
				xValues[i] * 5 +
				"," +
				(yValues[i] / (correct + incorrect)) * 100 +
				" ";
		}
	}
	$: console.log(coords);

	/**
	 * @param {{ keyCode: number; }} e
	 */
	function onKeyDown(e) {
		if (e.keyCode > 47 && e.keyCode < 58) {
			let inputNum = e.keyCode - 48;
			guess += inputNum.toString();
		} else if (e.keyCode == 8) {
			guess = guess.slice(0, -1);
		} else if (e.keyCode == 13) {
			handleSubmit();
		}
	}
	/**
	 * @param {number} timetable
	 */
	function toggleTimetable(timetable) {
		if (timetables.includes(timetable)) {
			const index = timetables.indexOf(timetable);
			timetables.splice(index, 1);
			timetables = timetables;
		} else {
			timetables.push(timetable);
			timetables = timetables;
		}
	}
</script>

<svelte:head>
	<title>Timetable Game</title>
</svelte:head>
<div>
	{#if ended}
		{createCoords()}
		<p>
			The game has ended! You got <span class="correct">{correct}</span>
			questions correct and <span class="incorrect">{incorrect}</span> questions
			incorrect!
		</p>
		<p>You got {percentage}% of questions correct!</p>
		<p>Your speed was {speed} seconds per question.</p>
		<p>{xValues}</p>
		<p>{yValues}</p>

		<svg
			viewBox="0 0 100 100"
			width="300"
			height="100"
			preserveAspectRatio="xMinYMin meet"
		>
			<!-- x axis -->
			<line x1="0" x2="300" y1="100" y2="100" />
			<g class="x" transform="translate(0,120)">
				<text x="0">0</text>
				<text x="300">60</text>
			</g>

			<!-- y axis -->
			<line x1="0" x2="0" y1="0" y2="100" />
			<g class="y" transform="translate(-10,0)">
				<text y="100">0</text>
				<text y="0">{correct + incorrect}</text>
			</g>

			<!-- data -->
			<polyline
				style="stroke: red; stroke-width: 2"
				fill="none"
				points={coords}
				class="line"
			/>
		</svg>
	{/if}
	<button on:click={toggleStart}>{open ? "End" : "Start"} Game</button>
	{#if open}
		<br /><span class="time">{seconds}</span>
		<Timer callback={handleTick} />
		<p class="question">What is {num1} x {num2}?</p>
		<span class="correct">{correct}</span>
		<span class="incorrect">{incorrect}</span><br />
		<input class="guess" bind:value={guess} disabled />
		<div id="keypad">
			<button on:click={() => (guess += "1")}>1</button>
			<button on:click={() => (guess += "2")}>2</button>
			<button on:click={() => (guess += "3")}>3</button>
			<button on:click={() => (guess += "4")}>4</button>
			<button on:click={() => (guess += "5")}>5</button>
			<button on:click={() => (guess += "6")}>6</button>
			<button on:click={() => (guess += "7")}>7</button>
			<button on:click={() => (guess += "8")}>8</button>
			<button on:click={() => (guess += "9")}>9</button>
			<button on:click={() => (guess = guess.slice(0, -1))}>Delete</button
			>
			<button on:click={() => (guess += "0")}>0</button>
			<button on:click={handleSubmit}>Enter</button>
		</div>
	{:else}
		<h3>Timetables that will appear</h3>
		{#each all_timetables as timetable}
			<button
				on:click={() => toggleTimetable(timetable)}
				class="timetable {timetables.includes(timetable)
					? ''
					: 'inactive'}">{timetable}</button
			>
		{/each}
	{/if}
</div>
<svelte:window on:keydown|preventDefault={onKeyDown} />

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
		border: 1px solid black;
		text-align: center;
		font-weight: bold;
		opacity: 1;
		background-color: white;
	}

	.timetable {
		color: white;
		margin: 5px;
	}

	.inactive {
		color: red;
		text-decoration: line-through;
	}

	svg {
		overflow: visible;
		margin: 3em;
	}

	line,
	polyline {
		fill: none;
		stroke: black;
	}

	.x text {
		text-anchor: middle;
	}

	.y text {
		text-anchor: end;
		dominant-baseline: middle;
	}

	.line {
		transform-origin: 50% 50%;
		/* center of rotation is set to the center of the element */
		transform: scale(1, -1);
	}
</style>
