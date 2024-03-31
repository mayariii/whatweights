<script lang="ts">
	import Calculator from '$lib/components/Calculator.svelte';
	import KoFi from '$lib/assets/ko-fi.png';
	import Accordion from '$lib/components/Accordion.svelte';
	let isKoFiOpen = false;

	const FAQS = [
		{
			question: 'what is whatweights? 🏋️‍♀️',
			answer:
				'whatweights is a web app designed to help weightlifters and gym-goers with barbell maths and calculating how much weight is on the bar or needed on the bar - this tool simplifies the process, making your workout setup quick and efficient!'
		},
		{
			question: 'how do I use whatweights?',
			answer:
				'firstly, select your barbell weight - then add the individual plates to see the total weight added, or enter a target weight and let the app calculate what plates you need!'
		},
		{
			question: 'are different barbell weights available?',
			answer:
				"yes, our calculator allows you to select the weight of your barbell, whether it's a standard 20kg bar, a 15kg women's bar, 7kg training bar, 6.4kg smith machine bar, or a 27kg trap bar - you also have the option to add 2.5kg competition collars."
		},
		{
			question: 'is whatweights free to use?',
			answer:
				'yes, whatweights is completely free to use! i created this tool to help myself and others, and i hope it makes your workout sessions more efficient and enjoyable :)'
		},
		{
			question: 'how can whatweights improve my workout routine?',
			answer:
				'by minimising the time spent calculating and figuring out weights, whatweights lets you focus more on your workout - this can lead to more effective training sessions and faster progress.'
		},
		{
			question: 'can I suggest features or improvements for whatweights?',
			answer:
				'feedback is always appreciated! If you have suggestions or ideas for improving whatweights, please send me an email at hey@mayari.io'
		}
	];
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
		name="keywords"
		content="Barbell Calculator, Weight Plate Calculator, Gym Workout Tools, Fitness Equipment Calculator, Barbell Plate Load, Workout Preparation App, Strength Training Assistant, Gym Training Aid" />

	<meta
		property="og:title"
		content="WhatWeights: simplify your lifting session with our barbell calculator!" />
	<meta property="og:url" content="https://www.whatweights.xyz" />

	<meta
		property="og:description"
		content="Discover the quickest way to calculate barbell plates for your workout with WhatWeights. Spend less time setting up and more time lifting!" />

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

	<div class="mt-10 flex flex-col gap-2 w-[300px]">
		{#each FAQS as { question, answer }}
			<Accordion>
				<h2 slot="head">{question}</h2>
				<p slot="details">{answer}</p>
			</Accordion>
		{/each}
	</div>
</div>
