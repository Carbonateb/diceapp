<script lang="ts">
	export let diceResults: number[];

	let revealed = false;

	$: {
		diceResults;
		hide();
	}

	function hide() {
		revealed = false;
	}
</script>

{#if diceResults.length > 1}
	<div class="py-4 text-lg">
		{#if revealed}
			<div class="rounded bg-slate-900 p-2 text-center font-bold text-slate-400">
				{diceResults.reduce((prev, current) => prev + current)}
			</div>
		{:else}
			<button
				class="w-full rounded bg-slate-900 p-2 text-slate-500"
				on:click={() => (revealed = true)}>Show sum</button
			>
		{/if}
	</div>
{/if}
