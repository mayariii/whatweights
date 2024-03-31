<script lang="ts">
	export let open = false;
	import { slide } from 'svelte/transition';
</script>

<button
	on:click={() => (open = !open)}
	class="p-2 border border-violet-100 text-gray-500 rounded-md w-full text-left text-xs font-medium">
	<slot name="head" />

	{#if open}
		<div transition:slide class="text-xs font-normal text-gray-400 mt-1">
			<slot name="details" />
		</div>
	{/if}
</button>
