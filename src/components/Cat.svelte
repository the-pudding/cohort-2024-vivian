<script>
	import { getContext } from "svelte";
	const copy = getContext("copy");
	import IPAGrid from "./IPAGrid.svelte";
	import IPABubbles from "./IPABubbles.svelte";
	import IPA from "$components/IPA.svelte";

	export let display = "grid"; // "grid" | "bubbles"
</script>

{#if display === "cover"}
	<div class="scrolly-cover">
		<img src={`assets/cat-face-logo.png`} width=125 height=125 alt={`cat face doodle`}/>
		<p class="scrolly-cover-hed">Cat</p>
	</div>
{:else if display === "grid"}
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
		<div><IPA ipa="miau" word=" " lang="english + 14 others" colors={copy.ipaColors.cat} audioSrc="cat-english" ipaScale=3/></div>
		<div><IPA ipa="ɲaː" word="ニャー" lang="japanese" colors={copy.ipaColors.cat} audioSrc="cat-japanese" ipaScale=3/></div>
	</div>
{:else if display === "grid-detail-3"}
{@const ipaObjects = copy.animals.cat.filter((d) => d?.patterns?.includes("detail-3"))}
<div class="two-col smaller-gap">
	<div style="padding: 2em 0 0 5em"><IPA ipa="miau" lang="english + 14 others" colors={copy.ipaColors.cat} audioSrc="cat-english" ipaScale=2.5/></div>
	<div class="flex-wrap">
		{#each ipaObjects as ipaObj}
		<div class="flex-child"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.cat} audioSrc={`cat-${ipaObj.lang}`} ipaScale=1.25/></div>
		{/each}
	</div>
</div>
{:else if display === "grid-detail-4"}
{@const ipaObjects = copy.animals.cat.filter((d) => d?.patterns?.includes("detail-4"))}
<div class="two-col small-gap">
	<div style="padding: 2em 0 0 8em"><IPA ipa="miau" lang="english + 14 others" colors={copy.ipaColors.cat} audioSrc="cat-english" ipaScale=2.5/></div>
	<div class="flex-wrap">
		{#each ipaObjects as ipaObj}
		<div class="flex-child"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.cat} audioSrc={`cat-${ipaObj.lang}`}/></div>
		{/each}
	</div>
</div>
{/if}

<style lang="scss">
</style>
