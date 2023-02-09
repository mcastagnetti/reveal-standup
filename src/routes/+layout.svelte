<script>
	import '../app.css';
</script>

<slot />

<style lang="postcss">
	:global(html) {
		height: 100vh;
		background: linear-gradient(to bottom, #be4bdb 5%, #9775fa 95%);
	}
</style>
