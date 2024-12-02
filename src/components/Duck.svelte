<script>
	import { getContext } from "svelte";
	const copy = getContext("copy");
	import IPAGrid from "./IPAGrid.svelte";
	import IPABubbles from "./IPABubbles.svelte";

	let display = "grid"; // "grid" | "bubbles"
</script>

{#if display === "grid"}
	<IPAGrid ipaObjects={copy.animals.duck} ipaColors={copy.ipaColors.duck} />
{:else if display === "bubbles"}
	<IPABubbles ipaObjects={copy.animals.duck} ipaColors={copy.ipaColors.duck} />
{/if}
