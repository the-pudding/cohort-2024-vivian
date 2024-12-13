<script>
	import { getContext } from "svelte";
	const copy = getContext("copy");
	import IPAGrid from "./IPAGrid.svelte";
	import IPABubbles from "./IPABubbles.svelte";

	export let display = "grid"; // "grid" | "bubbles"
</script>

{#if display === "grid"}
	<IPAGrid ipaObjects={copy.animals.cat} ipaColors={copy.ipaColors.cat} animal="cat"/>
{:else if display === "bubbles"}
	<IPABubbles ipaObjects={copy.animals.cat} ipaColors={copy.ipaColors.cat} animal="cat"/>
{/if}
