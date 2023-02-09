<script>
	import '../app.css';
</script>

<slot />

<style lang="postcss">
	:global(html) {
		min-height: 100vh;
		background: linear-gradient(to bottom, #be4bdb 5%, #9775fa 95%);
	}

	/* Avoid Chrome to see Safari hack */
	@supports (-webkit-touch-callout: none) {
		:global(html) {
			/* The hack for Safari */
			min-height: -webkit-fill-available;
		}
	}
</style>
