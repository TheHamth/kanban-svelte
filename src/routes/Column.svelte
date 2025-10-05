<script lang="ts">
	import { DiamondPlus, Check } from "@lucide/svelte";
	import { dndzone } from "svelte-dnd-action";
	import { flip } from "svelte/animate";

	let { column, onconsider, onfinalize } = $props();
	let showInput = $state(false);

	function hideInput(event: FocusEvent) {
		const currentTarget = event.currentTarget as HTMLElement;
		const relatedTarget = event.relatedTarget as Node | null;
		if (!currentTarget.contains(relatedTarget)) {
			showInput = false;
		}
	}

	function addTask(event: SubmitEvent) {
		event.preventDefault();
		const form = new FormData(event.currentTarget as HTMLFormElement);
		let newTask = (form.get("newTask") as string).trim();

		if (!newTask) return;

		column.tasks.push({ id: Date.now(), name: newTask });
		showInput = false;
	}

	const flipDurationMs = 100;
</script>

<div class="w-full bg-gray-100 p-4 rounded-lg shadow">
	<h2 class="font-bold mb-2">{column.title}</h2>
	<ul
		class="space-y-2.5"
		use:dndzone={{
			items: column.tasks,
			type: "tasks",
			dropTargetStyle: {},
			flipDurationMs,
		}}
		{onconsider}
		{onfinalize}
	>
		{#each column.tasks as task (task.id)}
			<li
				class="p-2 bg-white rounded shadow outline-none"
				animate:flip={{ duration: flipDurationMs }}
			>
				{task.name}
			</li>
		{/each}
		<li>
			{#if !showInput}
				<button
					class="p-2 bg-gray-100 rounded border border-gray-300 w-full flex justify-center items-center hover:scale-102 hover:cursor-pointer"
					onclick={() => (showInput = true)}
				>
					Add task <DiamondPlus class="mx-2" />
				</button>
			{:else}
				<form
					class="flex"
					method="POST"
					action="?/create"
					onfocusout={hideInput}
					onsubmit={addTask}
				>
					<!-- svelte-ignore a11y_autofocus -->
					<input
						name="newTask"
						type="text"
						class="p-2 bg-white rounded rounded-r-none shadow w-full border-none ring-0"
						autofocus
					/>
					<button
						class="px-1.5 bg-green-50 rounded rounded-l-none hover:cursor-pointer"
						type="submit"
						><Check color="green" />
					</button>
				</form>
			{/if}
		</li>
	</ul>
</div>
