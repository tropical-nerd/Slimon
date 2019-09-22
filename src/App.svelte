<script>

	const game = {
		on: false,
		round: 0, // integer
		boardActive: false,
		buttons: [ 
			{ name: '1', lit: false },
			{ name: '2', lit: false },
			{ name: '3', lit: false },
			{ name: '4', lit: false }],
		message: ''
	}

	let sequence = [];

	function startGame() {
		game.on = true;
		game.round = 0;
		game.message = '';
		newRound();
	}

	function newRound() {
		game.round += 1;
		playSequence();
	}

	function playSequence() {
		// Create sequence
		sequence = [];

		for (let sequenceIndex = 0; sequenceIndex < game.round; sequenceIndex++) {
			sequence[sequenceIndex] = Math.floor(Math.random() * game.buttons.length);
		}

		// Display sequence
		let sequenceIndex = 0;
		let playInterval = setInterval(() => {
			if (sequenceIndex + 1 === sequence.length) {
				playLightUp(sequence[sequenceIndex], true);
				clearInterval(playInterval);
			} else {
				playLightUp(sequence[sequenceIndex], false);
			}

			sequenceIndex += 1;
		}, 1000);
	}

	function buttonClick(buttonIndex) {
		if (game.boardActive === false) {
			return;
		}

		clickLightUp(buttonIndex);

		if (buttonIndex === sequence[0]) {
			sequence.shift();
			sequence = sequence;

		} else {
			gameOver();
		}
	}

	function playLightUp(buttonIndex, isLastOne = false) {
		console.log(buttonIndex);
		game.buttons[buttonIndex].lit = true;
		
		setTimeout(() => {
			game.buttons[buttonIndex].lit = false;

			if (isLastOne) {
				game.boardActive = true;
			}
		}, 500);
	}

	function clickLightUp(buttonIndex) {
		game.buttons[buttonIndex].lit = true;
		
		setTimeout(() => {
			game.buttons[buttonIndex].lit = false;
			
			if (sequence.length === 0) {
				game.boardActive = false;
				setTimeout(() => { newRound(); }, 500);
			}
		}, 500);
	}

	function gameOver() {
		game.message = 'Nope';
		game.boardActive = false;
		game.on = false;
	}

</script>

<style>
	.body {
		background-color: #e0e0e0;
	}

	.container {
		display: flex;
		flex-direction: column;
		max-width: 75vmin;
	}

	.game-board {
		display: grid;
		grid-template-columns: 30vmin 30vmin;
		grid-template-rows: 30vmin 30vmin;
		grid-gap: 5vmin;
		padding: 5vmin;
		margin-bottom: 1rem;
		border-radius: 32.5vmin;
		background-color: #111;
	}

	.game-button {
		width: 100%;
		height: 100%;
		font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
		font-weight: 600;
		font-size: 15vmin;
		border: none;
		transition: background-color .1s ease-out;
		color: rgba(0,0,0,0);
		box-shadow: inset 0 0 15px rgba(0,0,0,.5);
	}

	.game-button--0 {
		background-color: #880000;
		border-radius: 90% 10% 10% 10%;		
	}

	.game-button--0.lit {
		background-color: #ee3333;
	}

	.game-button--1 {
		background-color: #007700;
		border-radius: 10% 90% 10% 10%;
	}

	.game-button--1.lit {
		background-color: #33cc33;
	}

	.game-button--2 {
		background-color: #000099;
		border-radius: 10% 10% 10% 90%;
	}
	
	.game-button--2.lit {
		background-color: #3333ff;
	}

	.game-button--3 {
		background-color: #999900;
		border-radius: 10% 10% 90% 10%;
	}

	.game-button--3.lit {
		background-color: #dddd33;
	}

	.game-start {
		margin: 0 auto;
	}

</style>

<main class='container'>
	<h1>Slimon</h1>

	<div class="game-board">
		{#each game.buttons as button, index}
			<button
				class="game-button game-button--{index}"
				class:lit={button.lit}
				on:click="{() => buttonClick(index)}"
			>{button.name}</button>
		{/each}
	</div>

	{#if !game.on}
		<button class="game-start" on:click={startGame}>Start</button>
	{/if}

	{#if game.message.length > 0}
		<div>{game.message}</div>
	{/if}
</main>