<script lang="ts" context="module">
	export type Plate = {
		weight: number;
		size: string;
		colour: string;
		count: number;
	};
</script>

<script lang="ts">
	let barWeight = 15;
	let addedWeight = 0;
	let addedPlates: Plate[] = [];
	let platesNeeded: Plate[] = [];
	let targetWeight = barWeight;
	let showPlatesNeeded = false;

	let platesData = [
		{ weight: 25, size: 'large', colour: 'bg-red-600 text-white', count: 0 },
		{ weight: 20, size: 'large', colour: 'bg-blue-700 text-white', count: 0 },
		{ weight: 15, size: 'large', colour: 'bg-yellow-300', count: 0 },
		{ weight: 10, size: 'large', colour: 'bg-green-600 text-white', count: 0 },
		{ weight: 5, size: 'medium', colour: 'bg-white', count: 0 },
		{ weight: 2.5, size: 'small', colour: 'bg-red-600 text-white', count: 0 },
		{ weight: 2, size: 'small', colour: 'bg-blue-700 text-white', count: 0 },
		{ weight: 1.5, size: 'small', colour: 'bg-yellow-300', count: 0 },
		{ weight: 1.25, size: 'small', colour: 'bg-orange-500 text-white', count: 0 },
		{ weight: 1, size: 'small', colour: 'bg-green-600 text-white', count: 0 },
		{ weight: 0.5, size: 'small', colour: 'bg-white', count: 0 }
	];

	const BAR_WEIGHTS = [15, 20, 7, 6.4];

	$: minTargetWeight = barWeight;
	$: addedPlates = [];
	$: addedWeight = addedPlates.reduce((acc, { weight }) => acc + weight, 0);
	$: totalWeight = barWeight + addedWeight * 2;

	function calculatePlatesNeeded() {
		let remainingWeight = (targetWeight - barWeight) / 2; // divide by 2 to distribute weight evenly on each side

		platesData
			.sort((a, b) => b.weight - a.weight)
			.forEach((plate) => {
				while (remainingWeight >= plate.weight) {
					platesNeeded.push(plate);
					remainingWeight -= plate.weight;
				}
			});

		return platesNeeded;
	}
</script>

<div class="mt-6 flex flex-col items-center gap-6">
	<div class="flex gap-0.5 items-center min-h-32 relative">
		<div
			class="flex justify-center text-xs h-4 w-[350px] md:w-[500px] bg-gray-300 rounded absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2">
			{barWeight}kg
		</div>
		<!-- left side -->
		{#each addedPlates
			.slice()
			.sort((a, b) => b.weight - a.weight)
			.reverse() as plate}
			<div
				class:h-32={plate.size === 'large'}
				class:h-20={plate.size === 'medium'}
				class:h-16={plate.size === 'small'}
				class:w-6={plate.size === 'large'}
				class:w-4={plate.size !== 'large'}
				class={`${plate.colour} rounded z-10 text-xs border border-gray-600 flex items-center justify-center`}>
				<p class:rotate-90={plate.size !== 'large'}>
					{plate.weight}
				</p>
			</div>
		{/each}

		<div class="w-20 md:w-32"></div>

		<!-- right side -->
		{#each addedPlates.slice().sort((a, b) => b.weight - a.weight) as plate}
			<div
				class:h-32={plate.size === 'large'}
				class:h-20={plate.size === 'medium'}
				class:h-16={plate.size === 'small'}
				class:w-6={plate.size === 'large'}
				class:w-4={plate.size !== 'large'}
				class={`${plate.colour} rounded z-10 text-xs border border-gray-600 flex items-center justify-center`}>
				<p class:rotate-90={plate.size !== 'large'}>
					{plate.weight}
				</p>
			</div>
		{/each}
	</div>

	<div class="flex flex-col gap-2 items-center">
		<p class="text-sm text-gray-500">add plates</p>
		<div class="flex items-center justify-center gap-2 flex-wrap max-w-[360px] md:max-w-[380px]">
			{#each platesData as plate}
				<button
					class={`relative text-sm rounded-full border border-gray-600 ${plate.colour}`}
					class:size-20={plate.size === 'large'}
					class:size-16={plate.size === 'medium'}
					class:size-10={plate.size === 'small'}
					aria-label={`add ${plate.weight}kg plates to bar`}
					on:click={() => {
						addedWeight = addedWeight + plate.weight;
						platesData[platesData.indexOf(plate)].count += 2;
						addedPlates = [
							...addedPlates,
							{ colour: plate.colour, weight: plate.weight, size: plate.size, count: 0 }
						];
					}}>
					{plate.weight}
					{#if plate.count > 0}
						<span
							class="absolute -bottom-2 right-1/2 transform translate-x-2 bg-indigo-500 text-white rounded-full w-4 h-4 flex items-center justify-center text-xs">
							{plate.count}
						</span>
					{/if}
				</button>
			{/each}
		</div>
	</div>

	<div class="flex gap-8">
		<div class="flex flex-col items-center gap-2">
			<p class="text-sm text-gray-500">change bar</p>
			<div class="flex gap-1 justify-center *:border *:rounded-lg *:text-sm *:p-1">
				{#each BAR_WEIGHTS as weight}
					<button
						on:click={() => {
							barWeight = weight;
							showPlatesNeeded = false;
							targetWeight = barWeight;
							platesNeeded = [];
						}}
						class:bg-indigo-600={barWeight === weight}
						class:text-white={barWeight === weight}>
						{weight}kg
					</button>
				{/each}
			</div>
		</div>

		<div class="flex flex-col items-center gap-1">
			<p class="text-sm text-gray-500">total weight</p>
			<p class="text-5xl font-semibold">
				{totalWeight}<span class="ml-0.5 text-gray-400 text-2xl font-normal">kg</span>
			</p>
		</div>
	</div>

	<!-- reset button -->
	{#if addedWeight > 0}
		<button
			class="text-sm bg-gray-200 rounded px-1"
			on:click={() => {
				addedWeight = 0;
				barWeight = BAR_WEIGHTS[0];
				targetWeight = barWeight;
				addedPlates = [];
				platesData.forEach((plate) => (plate.count = 0));
			}}>empty bar</button>
	{/if}

	<div class="h-[1px] w-64 bg-gray-300" />

	{#if !showPlatesNeeded}
		<div class="flex flex-col gap-2">
			<label for="target-weight" class="text-sm text-gray-500">enter a target weight</label>
			<input
				id="target-weight"
				type="number"
				min={minTargetWeight}
				bind:value={targetWeight}
				class="text-center border rounded p-1 text-2xl font-semibold" />
		</div>
	{/if}

	{#if targetWeight > minTargetWeight && !showPlatesNeeded}
		<button
			class="bg-indigo-600 text-white text-sm rounded-md p-1"
			on:click={() => {
				platesNeeded = calculatePlatesNeeded();
				showPlatesNeeded = true;
			}}>show plates needed</button>
	{/if}

	{#if showPlatesNeeded}
		<div class="flex gap-0.5 items-center min-h-32 relative">
			<div
				class="flex justify-center text-xs h-4 w-[350px] md:w-[500px] bg-gray-300 rounded absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2">
				{barWeight}kg
			</div>
			<!-- left side -->
			{#each platesNeeded
				.slice()
				.sort((a, b) => b.weight - a.weight)
				.reverse() as plate}
				<div
					class:h-32={plate.size === 'large'}
					class:h-20={plate.size === 'medium'}
					class:h-16={plate.size === 'small'}
					class:w-6={plate.size === 'large'}
					class:w-4={plate.size !== 'large'}
					class={`${plate.colour} rounded z-10 text-xs border border-gray-600 flex items-center justify-center`}>
					<p class:rotate-90={plate.size !== 'large'}>
						{plate.weight}
					</p>
				</div>
			{/each}

			<div class="w-[100px] md:w-[200px]"></div>

			<!-- right side -->
			{#each platesNeeded.slice().sort((a, b) => b.weight - a.weight) as plate}
				<div
					class:h-32={plate.size === 'large'}
					class:h-20={plate.size === 'medium'}
					class:h-16={plate.size === 'small'}
					class:w-6={plate.size === 'large'}
					class:w-4={plate.size !== 'large'}
					class={`${plate.colour} rounded z-10 text-xs border border-gray-600 flex items-center justify-center`}>
					<p class:rotate-90={plate.size !== 'large'}>
						{plate.weight}
					</p>
				</div>
			{/each}
		</div>
	{/if}

	{#if showPlatesNeeded}
		<div class="flex flex-col items-center gap-1">
			<p class="text-sm text-gray-500">plates for target weight of</p>
			<p class="text-3xl font-semibold">
				{targetWeight}<span class="ml-0.5 text-gray-400 text-2xl font-normal">kg</span>
			</p>
		</div>
	{/if}

	<!-- reset button -->
	{#if showPlatesNeeded}
		<button
			class="text-sm bg-gray-200 rounded px-1 mb-10"
			on:click={() => {
				platesNeeded = [];
				targetWeight = barWeight;
				showPlatesNeeded = false;
			}}>reset</button>
	{/if}
</div>
