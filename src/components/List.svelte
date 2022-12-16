<script>
	import { onMount } from 'svelte';
	import { v4 as uuidv4 } from 'uuid';
	import Item from './Item.svelte';

	import { items } from '../stores';
	import TodoApi from '../TodoApi';
	import NewItem from './NewItem.svelte';

	function handleNewItem(e) {
		$items = [
			{
				id: uuidv4(),
				title: e.detail,
				complete: false
			},
			...$items
		];

		TodoApi.save($items);
	}

	function handleUpdateItem(e) {
		const index = $items.findIndex((item) => item.id === e.detail.id);
		$items[index] = e.detail;
		TodoApi.save($items);
	}

	function handleDeleteItem(e) {
		$items = $items.filter((item) => item.id !== e.detail);
		TodoApi.save($items);
	}

	let itemSorted = [];

	$: {
		itemSorted = [...$items];
		itemSorted.sort((a, b) => {
			if (a.complete && b.complete) return 0;
			if (a.complete) return 1;
			if (b.complete) return -1;
		});
	}

	onMount(async () => {
		localStorage.setItem('todo-list', JSON.stringify($items));
		$items = await TodoApi.getAll();
	});
</script>

<div class="list">
	<NewItem on:newitem={handleNewItem} />
	{#each itemSorted as item (item)}
		<Item {...item} on:update={handleUpdateItem} on:delete={handleDeleteItem} />
	{:else}
		<p class="list-status">No items found</p>
	{/each}
</div>

<style>
	.list {
		padding: 15px;
	}

	.list-status {
		margin: 15px 0;
		text-align: center;
		color: #ffffff;
		font-weight: bold;
		font-size: 1.1em;
	}
</style>
