<script>
	import { getContext } from "svelte";
	const copy = getContext("copy");
	import IPAGrid from "./IPAGrid.svelte";
	import IPABubbles from "./IPABubbles.svelte";

	let display = "grid"; // "grid" | "bubbles"
</script>

{#if display === "grid"}
	<IPAGrid ipaObjects={copy.animals.pig} ipaColors={copy.ipaColors.pig} />
{:else if display === "bubbles"}
	<IPABubbles ipaObjects={copy.animals.pig} ipaColors={copy.ipaColors.pig} />
{/if}
