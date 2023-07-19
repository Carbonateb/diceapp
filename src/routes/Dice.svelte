<script lang="ts">
	import { onMount } from "svelte";

	export let value = 1;
	export let maxValue = 6;
	export let index = 0;

	const animationDelayMs = 120;

	let rootDiv: HTMLDivElement;

	const isMaxed = value == maxValue;
	const isOne = value == 1;

	let suspense = false;
	let fakeOne = false;
	let fakeMaxed = false;

	/** A number between 0 and 1 */
	let rng = Math.random();

	if (isMaxed) {
		// To orange fakeouts
		// 10% chance of faking a One
		// 20% chance of faking a normal
		// 10% chance of faking a fakeout
		if (rng < 0.1) {
			suspense = true;
			fakeOne = true;
		} else if (rng < 0.3) {
			suspense = true;
		} else if (rng < 0.4) {
			suspense = true;
			fakeMaxed = true;
		}
	} else if (isOne) {
		// To grey fakeouts
		// 10% chance of faking a Max
		// 20% chance of faking a normal
		// 10% chance of faking a fakeout
		if (rng < 0.1) {
			suspense = true;
			fakeMaxed = true;
		} else if (rng < 0.3) {
			suspense = true;
		} else if (rng < 0.4) {
			suspense = true;
			fakeOne = true;
		}
	} else if (Math.random() < 1 / maxValue - 2) {
		// A "to white" fakeout has the same odds as getting a 1 or maxValue
		// To white fake outs
		// 33% chance of faking a Max
		// 33% chance of faking a one
		// 33% chance of faking a fakeout
		if (rng <= 0.333) {
			suspense = true;
			fakeMaxed = true;
		} else if (rng < 0.667) {
			suspense = true;
			fakeOne = true;
		} else {
			suspense = true;
		}
	}
</script>

<div
	bind:this={rootDiv}
	class="flex h-28 w-28 items-center justify-center rounded-full bg-orange-950/0 text-8xl font-medium"
	class:maxed={isMaxed}
	class:one={isOne}
	class:suspense
	class:fake-maxed={fakeMaxed}
	class:fake-one={fakeOne}
	style="animation-delay: {(2 + index) * animationDelayMs}ms;"
>
	{value}
</div>

<style lang="postcss">
	div {
		animation: appear 1200ms both cubic-bezier(0, -0, 0, 1);
		font-family: "Expletus Sans";
		--colour: theme(colors.slate.50);
	}

	.suspense {
		animation-name: appear-suspense;
		animation-duration: 2400ms; /* Double normal */
		--fakeout-color: theme(colors.slate.50);
	}

	.maxed {
		--colour: theme(colors.orange.500);
	}

	.one {
		--colour: theme(colors.slate.400);
	}

	.fake-maxed {
		--fakeout-color: theme(colors.orange.500);
	}

	.fake-one {
		--fakeout-color: theme(colors.slate.400);
	}

	@keyframes appear {
		0% {
			transform: translateY(50px) scale(0.5);
			opacity: 0;
			background-color: var(--colour);
			color: transparent;
		}
		25% {
			opacity: 1;
		}
		40%,
		70% {
			transform: translateY(0) scale(0.5);
			background-color: var(--colour);
			color: transparent;
		}
		71% {
			color: var(--colour);
		}
		100% {
			transform: translateY(0) scale(1);
			background-color: transparent;
		}
	}

	@keyframes appear-suspense {
		0% {
			transform: translateY(50px) scale(0.5);
			opacity: 0;
			background-color: var(--fakeout-color);
			color: transparent;
		}
		12.5% {
			opacity: 1;
		}
		20%,
		35% {
			transform: translateY(0) scale(0.5);
			background-color: var(--fakeout-color);
		}
		65% {
			background-color: var(--fakeout-color);
		}
		85% {
			transform: translateY(0) scale(0.7);
			background-color: var(--colour);
			color: transparent;
		}
		86% {
			color: var(--colour);
		}
		100% {
			transform: translateY(0) scale(1);
			background-color: transparent;
		}
	}
</style>
