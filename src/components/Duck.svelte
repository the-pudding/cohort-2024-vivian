<script>
	import { getContext } from "svelte";
	const copy = getContext("copy");
	import IPAGrid from "./IPAGrid.svelte";
	import IPABubbles from "./IPABubbles.svelte";

	export let display = "grid"; // "grid" | "bubbles"
</script>

{#if display === "grid"}
	<IPAGrid ipaObjects={copy.animals.duck} ipaColors={copy.ipaColors.duck} animal="duck"/>
{:else if display === "bubbles"}
	<IPABubbles ipaObjects={copy.animals.duck} ipaColors={copy.ipaColors.duck} />
{/if}
