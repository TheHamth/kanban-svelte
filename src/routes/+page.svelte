<script lang="ts">
	import Column from "./Column.svelte";

	type TaskColumn = {
		title: string;
		tasks: string[];
	};

	const columns: TaskColumn[] = $state([
		{
			title: "To Do",
			tasks: ["Add drag and drop feature", "Add persistence feature"],
		},
		{
			title: "In Progress",
			tasks: [
				"Finish the main page prototype",
				"Add create task feature",
			],
		},
		{ title: "Done", tasks: ["Start the project"] },
	]);
</script>

<div class="p-2 space-y-2 md:flex md:justify-around md:gap-7 md:p-8">
	{#each columns as column}
		<Column {column} />
	{/each}
</div>
