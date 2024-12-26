<script>
	import { getContext } from "svelte";
	const copy = getContext("copy");
	import IPAGrid from "./IPAGrid.svelte";
	import IPABubbles from "./IPABubbles.svelte";
	import IPA from "$components/IPA.svelte";

	export let display = "grid"; // "grid" | "bubbles"
</script>

{#if display === "grid"}
	<IPAGrid ipaObjects={copy.animals.cat} ipaColors={copy.ipaColors.cat} animal="cat"/>
{:else if display === "bubbles"}
	<IPABubbles ipaObjects={copy.animals.cat} ipaColors={copy.ipaColors.cat} animal="cat"/>
{:else if display === "grid-detail-1"}
	{@const ipaObjects = copy.animals.cat.filter((d) => d?.patterns?.includes("trend"))}
	<div class="two-col">
		<div style="padding-left: 9em"><IPA ipa="miau" lang="trend" colors={copy.ipaColors.cat} audioSrc="cat-english" ipaScale=2.5/></div>
		<div class="ipa-half-grid">
			{#each ipaObjects as ipaObj}
			<IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.cat} audioSrc={`cat-${ipaObj.lang}`} ipaScale=0.85/>
			{/each}
		</div>
	</div>
{:else if display === "grid-detail-2"}
	<div class="two-col small-gap">
		<div><IPA ipa="miau" word=" " lang="english + 13 others" colors={copy.ipaColors.cat} audioSrc="cat-english" ipaScale=2.5/></div>
		<div><IPA ipa="ɲaː" word="ニャー" lang="japanese" colors={copy.ipaColors.cat} audioSrc="cat-japanese" ipaScale=2.5/></div>
	</div>
{:else if display === "grid-detail-3"}
{@const ipaObjects = copy.animals.cat.filter((d) => d?.patterns?.includes("detail-3"))}
<div class="two-col">
	<div style="padding-left: 9em"><IPA ipa="miau" lang="trend" colors={copy.ipaColors.cat} audioSrc="cat-english" ipaScale=2.5/></div>
	<div class="ipa-half-grid">
		{#each ipaObjects as ipaObj}
		<IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.cat} audioSrc={`cat-${ipaObj.lang}`} ipaScale=0.85/>
		{/each}
	</div>
</div>
{:else if display === "grid-detail-4"}
{@const ipaObjects = copy.animals.cat.filter((d) => d?.patterns?.includes("detail-4"))}
<div class="two-col">
	<div style="padding-left: 9em"><IPA ipa="miau" lang="trend" colors={copy.ipaColors.cat} audioSrc="cat-english" ipaScale=2.5/></div>
	<div class="ipa-half-grid">
		{#each ipaObjects as ipaObj}
		<IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.cat} audioSrc={`cat-${ipaObj.lang}`} ipaScale=0.85/>
		{/each}
	</div>
</div>
{/if}

<style lang="scss">
	.ipa-half-grid {
		display: grid;
		justify-content: center;
		grid-template-columns: repeat(4, 1fr);
		grid-gap: 0em;
		padding: 1.5em 3em;
	}

	.two-col {
		display: flex;
		align-items: center;
		width: 100vw;
		justify-content: space-around;

		&.small-gap {
			justify-content: center;
			gap: 20em;
		}
	}
</style>
