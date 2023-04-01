<script lang="ts">
	import { flip } from 'svelte/animate';
	import { dndzone } from 'svelte-dnd-action'

	const flipDuartionMs = 100;

	interface ListItem {
		id: number
		title: string
		description: string
		tags: string[]
	}

	let items : ListItem[] = [
		{
			id: 1, // need to be unique
			title: "Blog post 1",
			description: "Blog post 1 description :)",
			tags: ["Cooper Codes"]
		},
		{
			id: 2, // need to be unique
			title: "Blog post 2",
			description: "Blog post 2 description :)",
			tags: ["Development", "Coding"]
		},
		{
			id: 3, // need to be unique
			title: "Blog post 3",
			description: "Make sure to subscribe and like the video.",
			tags: ["Subscribe"]
		}
	]

	const handleConsider = (evt : CustomEvent<DndEvent<ListItem>>) => {
		console.log("consider");
		// evt.detail.items <--
		items = evt.detail.items;
	}

	const handleFinalize = (evt: CustomEvent<DndEvent<ListItem>>) => {
		console.log("finalize");
		items = evt.detail.items;
	}

</script>

<div class="container h-full mx-auto flex justify-center items-center">
	<section
		use:dndzone="{{ items: items, flipDurationMs: flipDuartionMs, dropTargetStyle: {} }}"
		on:consider="{handleConsider}"
		on:finalize="{handleFinalize}"
	>
		{#each items as item (item.id)}
			<div class="card card-hover w-96 my-4" animate:flip="{{ duration: flipDuartionMs}}">
				<header class="card-header">
					<h4>{item.title}</h4>
				</header>
				<section class="p-4">
					{item.description}
				</section>
				<footer class="card-footer inline-block">
					{#each item.tags as tag}
						<span class="chip variant-filled-secondary">{tag}</span>
					{/each}
				</footer>
			</div>
		{/each}
	</section>
</div>
