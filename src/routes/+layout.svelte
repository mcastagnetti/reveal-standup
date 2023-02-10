<script>
	import '../app.css';
</script>

<main id="app">
	<slot />
</main>

<style lang="postcss">
	:global(:root) {
		background: linear-gradient(to bottom, #be4bdb 5%, #9775fa 95%);
	}

	#app {
		@apply min-h-screen flex flex-col items-center m-auto px-10 max-w-[500px] justify-between;
	}

	/* Avoid Chrome to see Safari hack */
	@supports (-webkit-touch-callout: none) {
		#app {
			/* The hack for Safari */
			min-height: -webkit-fill-available;
		}
	}
</style>
