<script>
	import '../app.css';
	/**
	 * @typedef {Object} Props
	 * @property {import('svelte').Snippet} [children]
	 */

	/** @type {Props} */
	let { children } = $props();
</script>

<svelte:head>
	<script
		defer
		src="https://eu.umami.is/script.js"
		data-website-id="fb04d224-2722-4289-866e-b74c22154c00"
		data-domains="whatweights.xyz,www.whatweights.xyz"></script>
</svelte:head>

<div class="h-screen w-full min-h-screen flex flex-col text-gray-700 bg-white">
	<main>
		{#if children}
			{@render children()}
		{/if}
	</main>

	<footer class="flex flex-col gap-2 mt-auto justify-center items-center p-4">
		<p class="text-xs text-gray-400">
			made by maya | <a href="https://mayari.io" target="_blank" class="hover:text-indigo-500"
				>mayari.io</a>
		</p>
	</footer>
</div>
