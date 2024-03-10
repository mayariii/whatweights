<script>
	import '../app.css';
</script>

<svelte:head>
	<script
		defer
		src="https://eu.umami.is/script.js"
		data-website-id="fb04d224-2722-4289-866e-b74c22154c00"
		data-domains="whatweights.xyz,www.whatweights.xyz"
	></script>
</svelte:head>

<div class="h-screen w-full min-h-screen flex flex-col text-gray-700 bg-white">
	<slot />
</div>
