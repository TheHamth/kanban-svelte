<script lang="ts">
	import { DiamondPlus } from "@lucide/svelte";

	const items = [
		{
			title: "To Do",
			tasks: [
				"Add create task feature",
				"Add drag and drop feature",
				"Add persistence feature",
			],
		},
		{ title: "In Progress", tasks: ["Finish the main page prototype"] },
		{ title: "Done", tasks: ["Start the project"] },
	];
</script>

<div class="p-2 space-y-2 md:flex md:justify-around md:gap-7 md:p-8">
	{#each items as item}
		<div class="w-full h-fit bg-gray-100 p-4 rounded-lg shadow">
			<h2 class="font-bold mb-2">{item.title}</h2>
			<ul class="space-y-2.5">
				{#each item.tasks as task}
					<li class="p-2 bg-white rounded shadow">{task}</li>
				{/each}
				<li
					class="p-2 bg-gray-100 rounded border border-gray-300 flex justify-center items-center hover:scale-102 hover:cursor-pointer"
				>
					Add task <DiamondPlus class="mx-2" />
				</li>
			</ul>
		</div>
	{/each}
</div>
