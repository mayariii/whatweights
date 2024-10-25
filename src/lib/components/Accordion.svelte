<script lang="ts">
	import { slide } from 'svelte/transition';
	
	// Interface to type the component's props
	interface Props {
		open?: boolean;
		head?: import('svelte').Snippet;
		details?: import('svelte').Snippet;
	}

	// Svelte 5 syntax for props and bindable values
	// $props() is a new way to declare component props
	// $bindable() creates a two-way binding for the 'open' prop
	let { open = $bindable(false), head, details }: Props = $props();
</script>

<button
	onclick={() => (open = !open)}
	class="p-2 border border-violet-100 text-gray-500 rounded-md w-full text-left text-xs font-medium">
	<!-- Svelte 5 syntax: {@render ...} is used to render snippets -->
	<!-- The ?. is optional chaining, which safely calls the snippet if it exists -->
	{@render head?.()}

	{#if open}
		<div transition:slide class="text-xs font-normal text-gray-400 mt-1">
			<!-- Another example of the new {@render ...} syntax -->
			{@render details?.()}
		</div>
	{/if}
</button>
