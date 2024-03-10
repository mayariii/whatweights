<script lang="ts">
	import Calculator from '$lib/components/Calculator.svelte';
</script>

<svelte:head>
	<title>what weights? | barbell plate calculator</title>
	<meta
		name="description"
		content="helping you spend less time figuring out the weights on the bar so you can get on with your reps ✨"
	/>
	<meta
		name="viewport"
		content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no"
	/>
	<meta
		property="og:image"
		content="https://firebasestorage.googleapis.com/v0/b/mayari-io.appspot.com/o/what-weights%2Fwhat-weights-OG.png?alt=media&token=513179ba-b9fd-4ae4-b908-acf1dc9b5553"
	/>
	<meta property="og:image:type" content="image/png" />
	<meta property="og:image:width" content="1200" />
	<meta property="og:image:height" content="630" />
</svelte:head>

<div class="h-full flex flex-col items-center p-4">
	<h1 class="font-semibold text-xl mt-4">whatweights.xyz 🏋️‍♀️</h1>
	<p class="text-xs text-gray-500 text-center mt-2 max-w-[40ch]">
		helping you spend less time figuring out the weights on the bar so you can get on with your
		reps ✨
	</p>
	<Calculator />


</div>
