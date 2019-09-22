<script>

	const game = {
		on: false,
		round: 0,
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
		// game.boardActive = false;

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
		/* display: grid;
		grid-template-columns: 30vmin 30vmin;
		grid-template-rows: 30vmin 30vmin;
		grid-gap: 5vmin; */
		padding: 5vmin;
		margin-bottom: 1rem;
		border-radius: 32.5vmin;
		width: 100%;
		height: 100%;
		/* background-color: #111; */
	}

	.button--0 {
		fill: #3C5;
	}
	.button--0.lit {
		fill: url(#e);
	}

	.button--1 {
		fill: #D34;
	}
	.button--1.lit {
		fill: url(#f);
	}
	.button--2 {
		fill: #EE4;
	}
	.button--2.lit {
		fill: url(#g);
	}

	.button--3 {
		fill: #48E;
	}
	.button--3.lit {
		fill: url(#h);
	}

	.game-start {
		margin: 0 auto;
	}

	.visibly-hidden {
		position: absolute;
		width: 1px;
		height: 1px;
		opacity: .01;
	}
</style>

<main class='container'>
	<h1 class="visibly-hidden">Slimon</h1>

	<!-- <div class="game-board">
		{#each game.buttons as button, index}
			<button
				class="game-button game-button--{index}"
				class:lit={button.lit}
				on:click="{() => buttonClick(index)}"
			>{button.name}</button>
		{/each}
	</div> -->

	<svg class="game-board" width="500" height="500" viewBox="0 0 132.292 132.292" aria-labelledby="game-board__title">
		<title id="game-board__title">Game Board</title>
		<defs>
			<linearGradient id="d"><stop offset="0" stop-color="#e7f0fe"/><stop offset=".393" stop-color="#e9f2fe"/><stop offset="1" stop-color="#87b5fa"/></linearGradient>
			<linearGradient id="c"><stop offset="0" stop-color="#fbfbdc"/><stop offset=".393" stop-color="#fbfbda"/><stop offset="1" stop-color="#f9f991"/></linearGradient>
			<linearGradient id="b"><stop offset="0" stop-color="#fdebec"/><stop offset=".393" stop-color="#fce0e3"/><stop offset="1" stop-color="#f3606e"/></linearGradient>
			<linearGradient id="a"><stop offset="0" stop-color="#e2f8e6"/><stop offset=".393" stop-color="#defce4" stop-opacity=".996"/><stop offset="1" stop-color="#5bf67d" stop-opacity=".996"/></linearGradient>
			<radialGradient xlink:href="#a" id="e" cx="36.622" cy="39.422" fx="36.622" fy="39.422" r="27.504" gradientTransform="matrix(1 0 0 .98211 0 .705)" gradientUnits="userSpaceOnUse"/>
			<radialGradient xlink:href="#b" id="f" cx="101.888" cy="40.414" fx="101.888" fy="40.414" r="27.538" gradientTransform="matrix(1 0 0 1.04578 0 -1.942)" gradientUnits="userSpaceOnUse"/>
			<radialGradient xlink:href="#c" id="g" cx="37.194" cy="100.17" fx="37.194" fy="100.17" r="29.372" gradientTransform="matrix(1 0 0 .89716 0 10.302)" gradientUnits="userSpaceOnUse"/>
			<radialGradient xlink:href="#d" id="h" cx="98.933" cy="97.711" fx="98.933" fy="97.711" r="27.142" gradientTransform="matrix(1 0 0 .95984 0 4.012)" gradientUnits="userSpaceOnUse"/>
		</defs>
		<path fill="#111" d="M66.542 6.431C15.54 11.685 5.253 49.552 3.94 67.282c-1.313 17.73 7.004 58.88 44.434 61.945 37.43 3.065 49.364-4.044 55.16-7.004 10.288-5.253 26.485-24.734 26.266-36.773-.218-12.04-3.94-50.563-14.227-60.194C105.285 15.625 89.744 5.993 66.542 6.43z" />
		<path class="button--0" class:lit={game.buttons[0].lit} on:click="{() => buttonClick(0)}" d="M39.619 18.908C18.605 31.166 12.695 47.363 10.507 53.492c-2.19 6.129-1.97 10.288 1.97 11.163 3.94.876 20.137 1.533 23.64 1.751 3.502.22 5.91-.875 7.88-3.064 1.97-2.189.4-8.289 3.94-12.914 3.54-4.626 8.536-6.567 11.382-7.005 2.845-.438 3.72-5.91 4.158-8.974.438-3.065 1.314-16.636-.218-19.7-1.533-3.064-8.756-4.378-23.64 4.159z" />
		<path class="button--1" class:lit={game.buttons[1].lit} on:click="{() => buttonClick(1)}" d="M70.044 17.376c.219 8.317.219 15.103 0 17.948-.219 2.846.438 6.348 3.502 7.661 3.065 1.314 13.79-1.094 17.292 3.94 3.503 5.035 5.035 18.606 4.816 20.138-.219 1.532 1.97 4.159 3.502 4.159 1.532 0 18.824-.438 21.232-.876 2.408-.437 5.254-.875 4.597-7.223-.657-6.348-3.721-24.515-14.884-35.022-11.164-10.506-30.207-14.884-33.71-14.446-3.501.437-5.91.656-6.347 3.72z" />
		<path class="button--2" class:lit={game.buttons[2].lit} on:click="{() => buttonClick(2)}" d="M35.898 74.505c-8.1-.437-12.696-.437-18.168-.656-5.472-.22-10.507.656-9.85 5.91.657 5.253 4.597 25.61 14.884 33.927 10.288 8.318 31.082 13.79 35.679 12.696 4.597-1.095 6.786-2.19 6.348-5.254-.438-3.064 1.313-25.61 1.75-29.33.439-3.722-5.034-1.095-11.82-3.94-4.873-2.044-10-4.094-12.161-9.753-.736-1.926-6.662-3.6-6.662-3.6z"/>
		<path class="button--3" class:lit={game.buttons[3].lit} on:click="{() => buttonClick(3)}" d="M77.486 90.046c-2.626.657-5.472 1.751-5.69 7.661-.22 5.91-.22 18.168.218 21.67.438 3.502 1.532 6.348 5.472 6.567 3.94.219 19.919-3.94 29.112-12.258 9.194-8.318 20.357-23.859 19.262-31.52-1.094-7.66-.656-8.317-6.785-8.317-6.129 0-15.76 1.532-17.511 1.97-1.751.437-4.816-.657-6.348 1.97-1.532 2.626-.875 5.91-4.378 8.755-3.502 2.846-8.536 3.065-13.352 3.502z"/>
		<path fill="#DDD" d="M75.147 70.917c-11.527-.3-24.25-.599-25.748-.599-1.497 0-4.49-.45-4.49-.45 0 .001 1.646-10.328 4.19-16.016 2.546-5.689 8.983-7.335 10.63-7.485 1.646-.15 20.358-.898 23.501 0 3.144.898 7.485 3.443 8.233 6.886.749 3.443 1.797 7.934 2.096 11.077.3 3.144.15 6.287.15 6.287s-9.73.15-18.562.3z" />
		<path fill="#111" d="M59.95 60.612a3.7 3.7 0 0 0-.241.002c-1.06.05-1.084.492-1.06.883-.047.815.095 4.343.048 4.98-.048.635-.286 1.374.665 1.45.95.077 2.85.077 3.61.077.761 0 .626-.713.626-.891 0-.178.176-.611-.632-.662-.808-.051-1.561.127-1.989.025-.427-.102-.149-4.495-.142-4.699.028-.96-.346-1.15-.885-1.165zm-7.145.034c-.726.003-1.372.073-1.758.24-1.235.534-1.14 2.342-.903 2.698.238.357 2.233.484 3.231.484.998 0 1.283-.076 1.71.204.428.28.346 2.183.156 2.386-.19.204-1.9.158-2.66.107-.576-.038.05-1.56-.807-1.645-.359-.036-.49-.117-1.229-.054-1.264.108-.888 1.565-.935 1.896-.048.33-.025.756 1.115.883 1.14.128 3.98.118 4.503-.035.523-.153 2.376-.382 2.423-1.07.048-.687.095-2.265-.142-2.723-.238-.458-1.853-.815-3.564-.866-1.71-.05-1.9-.33-1.9-.484 0-.152.086-1.06 1.378-1.069 1.894-.012.894.821 1.512 1.05.617.23 2.165.025 2.165.025s.458-1.227-.207-1.71c-.457-.333-2.49-.323-4.088-.317zm29.371.136a7.482 7.482 0 0 0-.382 0c-.19.005-.41.014-.671.027-1.093 0-2.376.128-2.423.866-.048.738-.048 4.607-.048 4.964 0 .356.333 1.044 1.14 1.069.808.026 3.232.076 3.707.05.475-.025 1.187-.432 1.092-.788-.095-.357-.332-2.724-.57-3.386-.237-.662-.19-.662-.19-.662s-.333-1.807-.808-1.985a2.433 2.433 0 0 0-.847-.155zm-15.497.002c-.333 0-.808.026-1.33.026-.618-.051-1.046.203-1.093.483-.048.28-.095 6.237.332 6.34.428.1.618.1 1.093.1s1.188-.076 1.52-.076c.333 0 .523-.33.475-.585-.047-.255-.617-5.422-.57-5.65.048-.23.048-.281.048-.281s-.143-.357-.475-.357zm2.376 0c-.713-.025-.523 1.019-.523 1.655 0 .637.142 4.923.333 5.229.19.305.76.203 1.472.203 1.283 0 .808-1.334.76-1.792-.047-.458.143-2.57.238-2.85.095-.28.285-.23.428-.256.142-.025.427.51.475.764.047.255-.285 2.343-.333 2.775-.047.433-.19 1.257.333 1.41.523.153 1.33.051 1.663.051.333 0 .38-.483.427-.687.048-.204.238-1.894.238-2.25 0-.356.903-1.63.903-1.63s.095.586 0 .891c-.095.306-.333 2.709-.428 2.912-.095.204 0 .611.238.688.237.076.808.025 1.33.025.523 0 .856-.458.903-.636.048-.178.237-1.843.237-2.276 0-.433-.142-3.08-.094-3.411.047-.331-.096-.713-.761-.713s-1.14-.025-1.615 0c-.476.026-.665.484-.665.484s-.143-.458-.428-.51c-.285-.05-1.473-.05-1.853.18-.264.158-.498.626-.62.904a.589.589 0 0 0 .003-.217c-.048-.28.047-.764-.38-.84-.428-.077-1.569-.077-2.281-.103zm16.012.128c-.523 0-.428.382-.38.662.047.28.094 1.603.094 2.443 0 .84.38 3.187.38 3.442 0 .255.76.433 1.236.458.95-.076.712-.535.712-1.018 0-.484-.19-2.678-.19-3.594 0-.917.903-1.044 1.188-.942.285.101.286 1.196.38 1.883.096.688.238 3.213.285 3.417.048.203.476.33.998.33.523 0 .998-.178 1.046-.382.047-.203-.143-2.244-.238-2.932-.095-.687-.475-3.055-.665-3.31-.19-.254-1.093-.279-1.9-.304-.809-.026-1.188.56-1.188.56s0-.535-.143-.637c-.142-.102-1.093-.076-1.615-.076zm-3.633 1.195c.21.035.273.646.408 2.07.144 1.52-.123 2.466-.288 2.488-.164.022-.555-.044-.575-2.113-.021-2.07.185-2.444.41-2.444a.12.12 0 0 1 .045 0z"/>
		<path class="button--start" class:lit={!game.on} on:click={startGame} d="M71.989 49.362c-1.632 0-5.081 0-5.712.074-.63.074-1.557.37-1.595.927-.037.556-.148 1.224-.11 1.632.036.408.593.63 1.334.668.742.037 6.49.185 8.086.148 1.594-.037 2.522-.037 2.781-.037.26 0 1.261 0 1.298-.742.037-.742 0-1.595-.037-1.929-.037-.333-.37-.63-1.187-.667-.815-.037-3.82-.074-4.858-.074z" />
	</svg>

	{#if game.message.length > 0}
		<div>{game.message}</div>
	{/if}
</main>