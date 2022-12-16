<script>
	import { createEventDispatcher } from 'svelte';

	export let id, title, complete;

	const dispatcher = createEventDispatcher();

	function triggerUpdate() {
		dispatcher('update', { id, title, complete });
	}

	function handleDoubleClick() {
		const yes = confirm('Are you sure wish to delete this item?');

		if (yes) {
			dispatcher('delete', id);
		}
	}
</script>

<div class="item" class:complete on:dblclick={handleDoubleClick}>
	<input
		type="text"
		class="text-input"
		bind:value={title}
		readonly={complete}
		on:keyup={({ key, target }) => key === 'Enter' && target.blur()}
		on:blur={() => triggerUpdate()}
	/>
	<input
		type="checkbox"
		class="completed-checkbox"
		bind:checked={complete}
		on:change={() => triggerUpdate()}
	/>
</div>

<style>
	.item {
		display: flex;
		align-items: center;
		padding: 15px;
		background: #ffffff;
		border-radius: 10px;
		margin-bottom: 15px;
	}

	.item.complete {
		background: #dddddd;
	}

	.item.complete .text-input {
		color: #555555;
		text-decoration: line-through;
	}

	.item:focus-within {
		background: rgba(255, 255, 255, 0.8);
	}

	.text-input {
		flex-grow: 1;
		background: none;
		border: none;
		outline: none;
		font-weight: 500;
		font-size: 1em;
	}

	.completed-checkbox {
		margin-left: 15px;
	}
</style>
