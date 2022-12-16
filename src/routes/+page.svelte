<script>
	import Header from '../components/Header.svelte';
	import List from '../components/List.svelte';
</script>

<div class="container">
	<Header />
	<List />
</div>

<style>
	.container {
		width: 500px;
		background: #40b3ff;
		margin: 30px auto;
		border-radius: 10px;
	}

	:global(.container *) {
		font-family: 'Poppins', sans-serif;
	}
</style>
