<script lang="ts">
	import Column from "./Column.svelte";

	type Task = {
		id: number;
		name: string;
	};

	type TaskColumn = {
		title: string;
		tasks: Task[];
	};

	const columns: TaskColumn[] = $state([
		{
			title: "To Do",
			tasks: [{ id: 1, name: "Add edit and delete tasks feature" }],
		},
		{
			title: "In Progress",
			tasks: [
				{ id: 2, name: "Finish the main page prototype" },
				{ id: 3, name: "Add persistence feature" },
			],
		},
		{
			title: "Done",
			tasks: [
				{ id: 4, name: "Start the project" },
				{ id: 5, name: "Add create task feature" },
				{ id: 6, name: "Add drag and drop feature" },
			],
		},
	]);

	function handleDndConsider(index: number, event: CustomEvent) {
		columns[index].tasks = event.detail.items;
	}
	function handleDndFinalize(index: number, event: CustomEvent) {
		columns[index].tasks = event.detail.items;
	}
</script>

<div
	class="p-2 space-y-2 md:flex md:items-start md:justify-around md:gap-7 md:p-8"
>
	{#each columns as column, i}
		<Column
			{column}
			onconsider={(e: CustomEvent) => handleDndConsider(i, e)}
			onfinalize={(e: CustomEvent) => handleDndFinalize(i, e)}
		/>
	{/each}
</div>
