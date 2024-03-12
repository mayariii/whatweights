<script lang="ts">
	import Calculator from '$lib/components/Calculator.svelte';
	import KoFi from '$lib/assets/ko-fi.png';
	let isKoFiOpen = false;
</script>

<svelte:head>
	<title>what weights? | barbell plate calculator</title>
	<meta
		name="description"
		content="helping you spend less time figuring out the weights on the bar so you can get on with your reps ✨" />
	<meta
		name="viewport"
		content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
	<meta
		property="og:image"
		content="https://firebasestorage.googleapis.com/v0/b/mayari-io.appspot.com/o/what-weights%2Fwhat-weights-OG.png?alt=media&token=513179ba-b9fd-4ae4-b908-acf1dc9b5553" />
	<meta property="og:image:type" content="image/png" />
	<meta property="og:image:width" content="1200" />
	<meta property="og:image:height" content="630" />
</svelte:head>

<div class="h-full flex flex-col items-center p-4">
	<h1 class="font-semibold text-xl mt-4">whatweights.xyz 🏋️‍♀️</h1>
	<p class="text-xs text-gray-500 text-center mt-2 max-w-[40ch]">
		helping you spend less time figuring out the weights on the bar so you can get on with your reps
		✨
	</p>
	<Calculator />

	<div class="flex flex-col gap-3 items-center mt-12">
		<button on:click={() => (isKoFiOpen = !isKoFiOpen)}
			><img src={KoFi} alt="Support me on kofi" class="w-52" /></button>
		{#if isKoFiOpen}
			<a href="https://ko-fi.com/mayariii" target="_blank" class="text-sm text-indigo-500"
				>ko-fi.com/mayariii</a>
			<iframe
				id="kofiframe"
				src="https://ko-fi.com/mayariii/?hidefeed=true&widget=true&embed=true&preview=true"
				style="border:none;width:100%;padding:4px;background:#f9f9f9;"
				height="630"
				title="mayariii"></iframe>
			<button on:click={() => (isKoFiOpen = false)} class="text-sm">close</button>
		{/if}

		<p class=" text-gray-400 text-xs text-center italic">feedback: hey@mayari.io</p>
	</div>
</div>
