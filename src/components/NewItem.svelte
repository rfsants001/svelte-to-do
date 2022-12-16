<script>
	import { createEventDispatcher } from 'svelte';

	const dispatcher = createEventDispatcher();

	let value = '';
	function handleKeyUp({ key }) {
		if (key !== 'Enter' || value.length === 0) return;

		dispatcher('newitem', value);

		value = '';
	}
</script>

<input type="text" on:keyup={handleKeyUp} bind:value placeholder="+ Add Item" />

<style>
	input {
		box-sizing: border-box;
		margin-bottom: 15px;
		padding: 15px;
		border: none;
		outline: none;
		width: 100%;
		background: rgba(0, 0, 0, 0.25);
		color: #ffffff;
		font-size: 1em;
		font-weight: 600;
		cursor: pointer;
	}

	input::placeholder {
		color: #ffffff;
	}
</style>
