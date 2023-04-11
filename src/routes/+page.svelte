<script lang="ts">
	const MEMBERS = [
		'Tanguy Moreau',
		'Frédéric Laudarin',
		'Donia Nefiz',
		'Emeric Lemoine',
		'Marc-Olivier Castagnetti',
		'Damien Peelman',
	];

	const APIS = ['https://api.chucknorris.io/jokes/random'];

	const THEMES = {
		sandstorm: {
			song: 'Darude - Sandstorm.mp3',
			gif: 'cat-vibing.gif',
			alt: 'Cat Vibin',
			weight: 1,
		},
		bob: {
			song: 'Bob Marley - Get Up Stand Up.mp3',
			gif: 'cat-vibing.gif',
			alt: 'Cat Vibin',
			weight: 1,
		},
		cena: {
			song: 'John Cena - His Name is John Cena.mp3',
			gif: 'john-cena.gif',
			alt: 'John Cena',
			weight: 0.5,
		},
	} as const;

	function random<T>(items: T[]): T {
		const randomIndex = Math.floor(Math.random() * items.length);
		return items[randomIndex];
	}

	async function getJoke() {
		const data = await (await fetch(random(APIS))).json();

		return data.value.replaceAll('Chuck Norris', random(MEMBERS));
	}

	let isPlaying = false;
	let audio: HTMLAudioElement;
	let theme: (typeof THEMES)[keyof typeof THEMES];

	function getRandomTheme() {
		let weighthedArray: string[] = [];

		Object.keys(THEMES).forEach((themeName) => {
			weighthedArray = [
				...weighthedArray,
				Array(THEMES[themeName as keyof typeof THEMES].weight * 10).fill(theme),
			].flat();
		});

		return THEMES[weighthedArray[Math.floor(Math.random() * weighthedArray.length)] as keyof typeof THEMES];
	}

	function startPlaying(event: MouseEvent) {
		event.preventDefault();
		theme = getRandomTheme();

		isPlaying = true;
		audio = new Audio(`./musics/${theme.song}`);
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
		<div class="flex items-center flex-1">
			<img src="./gifs/{theme.gif}" alt={theme.alt} class="w-full max-h-full" />
		</div>
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
