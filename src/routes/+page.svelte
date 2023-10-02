<script lang="ts">
	import { onMount } from "svelte";
	import Dice from "./Dice.svelte";
	import { browser } from "$app/environment";
	import ResultsStats from "./ResultsStats.svelte";

	let numDice = 1;
	let numSides = 6;

	let diceResults: number[] = [];
	$: hasResults = diceResults.length > 0;

	function rollDice() {
		diceResults = [];

		sessionStorage.setItem("numDice", numDice.toString());
		sessionStorage.setItem("numSides", numSides.toString());

		for (let i = 0; i < numDice; i++) {
			diceResults.push(1 + Math.floor(Math.random() * numSides));
		}
	}

	onMount(() => {
		let savedNumDice = sessionStorage.getItem("numDice");
		if (savedNumDice) {
			numDice = parseInt(savedNumDice);
		}

		let savedNumSides = sessionStorage.getItem("numSides");
		if (savedNumSides) {
			numSides = parseInt(savedNumSides);
		}
	});
</script>

<svelte:head>
	<title>Dice App</title>
</svelte:head>

<main
	class="h-[100svh] select-none bg-slate-900 bg-gradient-to-b from-slate-900 to-slate-950 p-4 text-slate-50"
>
	<div class="mx-auto flex h-full max-h-full max-w-sm flex-col">
		{#if hasResults}
			<!-- Dice results -->
			<div class="flex flex-grow flex-wrap content-center justify-center gap-4">
				{#key diceResults}
					{#each diceResults as diceValue, index}
						<Dice value={diceValue} {index} maxValue={numSides} />
					{/each}
				{/key}
			</div>
			<ResultsStats {diceResults} />
		{:else}
			<div
				class="flex flex-grow flex-col items-center justify-center text-center"
				style="font-family: 'Expletus Sans';"
			>
				<span class="text-5xl"
					>Dice App <span class="text-lg text-orange-500">by Carbonateb</span>
				</span>
				<span class="text-slate-400">Sveltified</span>
			</div>
		{/if}

		<div class="flex flex-col gap-2">
			<button class="mb-4 h-12 rounded-lg bg-slate-700 text-3xl" on:click={rollDice}
				>Roll <span class="font-bold">{numDice}d{numSides}</span></button
			>
			<label>
				<div class="text-sm font-medium text-gray-400">{numDice} Dice</div>
				<input class="w-full" type="range" min="1" max="10" bind:value={numDice} />
			</label>
			<label>
				<div class="text-sm font-medium text-gray-400">{numSides} Sides</div>
				<input class="w-full" type="range" min="4" max="24" bind:value={numSides} />
			</label>
		</div>
	</div>
</main>
