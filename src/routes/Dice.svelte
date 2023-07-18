<script lang="ts">
	import { onMount } from "svelte";

	export let value = 1;
	export let maxValue = 6;
	export let index = 0;

	const animationDelayMs = 120;

	let rootDiv: HTMLDivElement;
</script>

<div
	bind:this={rootDiv}
	class="flex h-28 w-28 items-center justify-center rounded-full bg-orange-950/0 text-8xl font-medium"
	class:maxed={value == maxValue}
	style="animation-delay: {(2 + index) * animationDelayMs}ms;"
>
	{value}
</div>

<style lang="postcss">
	div {
		animation: appear 1200ms both cubic-bezier(0, -0, 0, 1);
		font-family: "Expletus Sans";
		--colour: theme(colors.slate.50);
		--glow-colour: transparent;
	}

	div.maxed {
		--colour: theme(colors.orange.500);
		--glow-colour: theme(colors.orange.600);
	}

	@keyframes appear {
		0% {
			transform: translateY(50px) scale(0.5);
			opacity: 0;
			background-color: var(--colour);
			color: transparent;
			box-shadow: 0 0 0px var(--glow-colour);
		}
		25% {
			opacity: 1;
		}
		40%,
		70% {
			transform: translateY(0) scale(0.5);
			background-color: var(--colour);
			color: transparent;
			box-shadow: 0 0 25px var(--glow-colour);
		}
		71% {
			color: var(--colour);
		}
		100% {
			transform: translateY(0) scale(1);
			background-color: transparent;

			box-shadow: 0 0 0px var(--glow-colour);
		}
	}
</style>
