<script lang="ts" context="module">
	export type Plate = {
		weight: number;
		size: string;
		colour: string;
		count?: number;
	};
</script>

<script lang="ts">
	let barWeight = 15;
	let targetWeight = barWeight;
	let addedWeight = 0;
	let unaccountedWeight = 0;
	let addedPlates: Plate[] = [];
	let platesNeeded: Plate[] = [];
	let showPlatesNeeded = false;
	let hasCompetitionCollars = false;

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

	const BAR_WEIGHTS = [15, 20, 7, 6.4, 27];

	$: minTargetWeight = barWeight;
	$: addedPlates = [];
	$: addedWeight = addedPlates.reduce((acc, { weight }) => acc + weight, 0);
	$: totalWeight = barWeight + addedWeight * 2 + (hasCompetitionCollars ? 5 : 0); // competition collars are 2.5kg each

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

		// unaccountedWeight is the remaining weight that can't be made up by the plates
		unaccountedWeight =
			targetWeight - barWeight - platesNeeded.reduce((acc, plate) => acc + plate.weight, 0) * 2;

		return platesNeeded;
	}
</script>

<div class="mt-6 flex flex-col items-center gap-6">
	<!-- barbell -->
	<div class="flex gap-0.5 items-center min-h-32 relative">
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

		<!-- gap between weights -->
		<div class="w-20 md:w-32" />

		<p
			class="flex justify-center text-xs h-4 w-[350px] md:w-[500px] bg-gray-300 rounded absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2">
			{barWeight}kg
		</p>

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

	<!-- plate selection -->
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
							{ colour: plate.colour, weight: plate.weight, size: plate.size }
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
	<div class="flex flex-col gap-4">
		<div class="flex gap-6">
			<!-- bar weight selector -->
			<div class="flex flex-col items-center justify-between gap-2">
				<p class="text-sm text-gray-500">change bar</p>
				<div
					class="flex flex-wrap w-[150px] gap-1 justify-center *:border *:rounded-lg *:text-sm *:p-1">
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

			<!-- total weight -->
			<div class="flex flex-col items-center gap-1">
				<p class="text-sm text-gray-500">total weight</p>
				<p class="text-5xl font-semibold tabular-nums">
					{totalWeight}<span class="ml-0.5 text-gray-400 text-2xl font-normal">kg</span>
				</p>
			</div>
		</div>

		<!-- toggle 2.5kg competition collars -->
		<div class="flex justify-center items-center gap-1">
			<label for="competition-collars" class="text-xs text-gray-500"
				>add competition collars (2.5kg each)</label>
			<input
				id="competition-collars"
				type="checkbox"
				class="size-4 rounded-full"
				on:change={() => {
					hasCompetitionCollars = !hasCompetitionCollars;
				}} />
		</div>
	</div>

	<!-- reset button -->
	{#if addedWeight > 0}
		<button
			class="text-sm bg-gray-200 rounded px-1"
			on:click={() => {
				addedWeight = 0;
				unaccountedWeight = 0;
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
		<!-- barbell 2 -->
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

			<!-- gap between weights -->
			<div class="w-[100px] md:w-[200px]" />

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

		<div class="flex flex-col items-center gap-1">
			<p class="text-sm text-gray-500">plates for target weight of</p>

			{#if unaccountedWeight}
				<p class="text-3xl font-semibold">
					{targetWeight - unaccountedWeight}<span class="ml-0.5 text-gray-400 text-2xl font-normal"
						>kg</span>
				</p>
				<p class="text-xs text-gray-400">
					remaining {unaccountedWeight.toFixed(1)}kg is unplateable
				</p>
			{:else}
				<p class="text-3xl font-semibold">
					{targetWeight}<span class="ml-0.5 text-gray-400 text-2xl font-normal">kg</span>
				</p>
			{/if}
		</div>
	{/if}

	<!-- reset button -->
	{#if showPlatesNeeded}
		<button
			class="text-sm bg-gray-200 rounded px-1 mb-10"
			on:click={() => {
				platesNeeded = [];
				targetWeight = barWeight;
				unaccountedWeight = 0;
				showPlatesNeeded = false;
			}}>reset</button>
	{/if}
</div>
