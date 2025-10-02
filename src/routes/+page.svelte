<script lang="ts">
	import { DiamondPlus, Check } from "@lucide/svelte";

	type TaskColumn = {
		title: string;
		tasks: string[];
		showInput: boolean;
	};

	const items: TaskColumn[] = $state([
		{
			title: "To Do",
			tasks: ["Add drag and drop feature", "Add persistence feature"],
			showInput: false,
		},
		{
			title: "In Progress",
			tasks: [
				"Finish the main page prototype",
				"Add create task feature",
			],
			showInput: false,
		},
		{ title: "Done", tasks: ["Start the project"], showInput: false },
	]);

	// svelte-ignore non_reactive_update
	let newTask = "";

	function addTask(event: SubmitEvent, item: TaskColumn) {
		event.preventDefault();
		item.showInput = false;
		item.tasks.push(newTask.trim());
		newTask = "";
	}

	function hideInput(event: FocusEvent, item: TaskColumn) {
		const currentTarget = event.currentTarget as HTMLElement;
		const relatedTarget = event.relatedTarget as Node | null;
		if (!currentTarget.contains(relatedTarget)) {
			item.showInput = false;
		}
	}
</script>

<div class="p-2 space-y-2 md:flex md:justify-around md:gap-7 md:p-8">
	{#each items as item}
		<div class="w-full h-fit bg-gray-100 p-4 rounded-lg shadow">
			<h2 class="font-bold mb-2">{item.title}</h2>
			<ul class="space-y-2.5">
				{#each item.tasks as task}
					<li class="p-2 bg-white rounded shadow">{task}</li>
				{/each}
				<li>
					{#if !item.showInput}
						<button
							class="p-2 bg-gray-100 rounded border border-gray-300 w-full flex justify-center items-center hover:scale-102 hover:cursor-pointer"
							onclick={() => (item.showInput = true)}
						>
							Add task <DiamondPlus class="mx-2" />
						</button>
					{:else}
						<form
							class="flex"
							onsubmit={(e: SubmitEvent) => addTask(e, item)}
							onfocusout={(e: FocusEvent) => hideInput(e, item)}
						>
							<!-- svelte-ignore a11y_autofocus -->
							<input
								bind:value={newTask}
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
	{/each}
</div>
