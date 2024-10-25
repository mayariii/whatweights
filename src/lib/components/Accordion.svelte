<script lang="ts">
	import { slide } from 'svelte/transition';
	interface Props {
		open?: boolean;
		head?: import('svelte').Snippet;
		details?: import('svelte').Snippet;
	}

	let { open = $bindable(false), head, details }: Props = $props();
</script>

<button
	onclick={() => (open = !open)}
	class="p-2 border border-violet-100 text-gray-500 rounded-md w-full text-left text-xs font-medium">
	{@render head?.()}

	{#if open}
		<div transition:slide class="text-xs font-normal text-gray-400 mt-1">
			{@render details?.()}
		</div>
	{/if}
</button>
