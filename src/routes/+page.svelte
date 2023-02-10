<script lang="ts">
	const SONGS = ['Darude - Sandstorm.mp3', 'Bob Marley - Get Up Stand Up.mp3'];

	async function getJoke() {
		const data = await (await fetch('https://api.chucknorris.io/jokes/random')).json();

		return data.value;
	}

	let isPlaying = false;
	let audio: HTMLAudioElement;

	function startPlaying(event: MouseEvent) {
		event.preventDefault();

		const randomSongIndex = Math.floor(Math.random() * SONGS.length);
		isPlaying = true;
		audio = new Audio(`./${SONGS[randomSongIndex]}`);
		audio.play();
	}

	function stopPlaying(event: MouseEvent) {
		event.preventDefault();
		audio.pause();

		isPlaying = false;
	}
</script>

<header class="mt-5">
	<img src="./logo.png" alt="Reveal's logo" />
</header>

<main class="h-full w-full flex flex-col items-stretch justify-between py-5 gap-10 flex-1">
	{#if isPlaying}
		<img src="./cat-vibing.gif" alt="Cat vibing" class="w-full max-h-full" />
		<button class="uppercase" on:click={stopPlaying}>WTF STOP IT</button>
	{:else}
		<article class="flex-1 flex items-center">
			{#await getJoke()}
				<p>Fetching juicy stuff...</p>
			{:then joke}
				<p>{joke}</p>
			{/await}
		</article>
		<button class="uppercase" on:click={startPlaying}>Stand up</button>
	{/if}
</main>

<style lang="postcss">
	p {
		@apply font-mono text-yellow-300;
		text-shadow: red 2px 3px;
	}

	button {
		@apply w-full p-5 rounded-xl text-2xl text-amber-400;

		--bg-size: 400%;

		text-shadow: red 2px 5px;

		background: linear-gradient(
			90deg,
			rgba(255, 0, 0, 1),
			rgba(255, 154, 0, 1),
			rgba(208, 222, 33, 1),
			rgba(79, 220, 74, 1),
			rgba(63, 218, 216, 1),
			rgba(47, 201, 226, 1),
			rgba(28, 127, 238, 1),
			rgba(95, 21, 242, 1),
			rgba(186, 12, 248, 1),
			rgba(251, 7, 217, 1),
			rgba(255, 0, 0, 1)
		);
		background-size: 200% 200%;

		animation: move-bg 15s linear infinite;

		@keyframes move-bg {
			to {
				background-position: var(--bg-size) 0;
			}
		}
	}
</style>
