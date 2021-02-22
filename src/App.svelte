<script>
	import ColorTest from './components/ColorTest.svelte';
	import Boxes from './components/Boxes.svelte';

	let start = false;
	let stop = false;
	let krok = 0;
	let points = 0;
	let max = 15;
	let correct;
	let newCorrect;
	let time;

	const startTest = () => {
		start = true;
		correct = Math.floor(Math.random() * 4);
		time = new Date().getTime();
	};

	const checkNewCorrect = () => {
		newCorrect = correct;
		if (krok % 4 < 2) {
			while(newCorrect == correct) {
				newCorrect = Math.floor(Math.random() * 4);
			}
		} else {
			while(newCorrect == correct) {
				newCorrect = Math.floor(Math.random() * 3);
			}
		}
		correct = newCorrect;
	}

	const handleNextItem = (event) => {
		if (event.detail == correct) {
			points++;
		}
		if (++krok == max) {
			let end = new Date().getTime();
			time = end - time;
			start = false;
			stop = true;
		}
		checkNewCorrect();
	}

	const retry = () => {
		start = false;
		stop = false;
		krok = 0;
		points = 0;
	}
</script>

<main>
	{#if !start && !stop}
		<div class="starting">
			<p><strong>Inštrukcie:</strong><br>
				Po spustení testu sa na obrazovke objaví farebne vyznačený text. Zakliknite farbu textu, nie to čo text znamená.<br>
				V druhej časti testu sa objaví obdĺžnik s textom umiestneným v strede, naľavo alebo napravo. Tu treba označiť čo sa v texte nachádza, nie kde sa text nachádza.</p>
			<button id="start_button" on:click={startTest}></button>
		</div>
	{:else if start}
		{#if krok % 4 < 2}
			<ColorTest {correct} 
			on:nextColor={handleNextItem} />
		{:else}
			<Boxes {correct} 
				on:nextBox={handleNextItem} />
		{/if}
	{:else}
		<div class="end">
			<span><strong>Skóre: </strong>{points}/{max}</span><br>
			<span><strong>Čas: </strong>{time / 1000} s</span><br>
			<button id="retry_button" on:click={retry}></button>
		</div>
	{/if}
</main>

<style lang="scss">
	:global{
		@import './style/style';
	}

	main {
		.starting {
			#start_button { 
				background-image: url('./../assets/start_icon.png');
				height: 70px;
				width: 70px;
				margin-top:1em;
				background-position: center;
				background-size: cover;
				background-color: transparent;
				border: none;
				cursor: pointer;
			}
		}

		#retry_button {
			background-image: url('./../assets/retry.png');
			height: 60px;
			width: 60px;
			margin-top: 2em;
			background-position: center;
			background-size: cover;
			background-color: transparent;
			border: none;
			cursor: pointer;
		}
	}
</style>