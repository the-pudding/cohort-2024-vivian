<script>
	import { getContext } from "svelte";
	const copy = getContext("copy");
	import IPAGrid from "./IPAGrid.svelte";
	import IPABubbles from "./IPABubbles.svelte";
	import IPA from "$components/IPA.svelte";

	export let display = "grid"; // "grid" | "bubbles"
	export let pageColors;
</script>

{#if display === "cover"}
	<div class="scrolly-cover">
		<img src={`assets/pig-face-logo.png`} width=125 height=125 alt={`pig face doodle`}/>
		<p class="scrolly-cover-hed">Pig</p>
	</div>
{:else if display === "grid"}
	<IPAGrid ipaObjects={copy.animals.pig} ipaColors={copy.ipaColors.pig} animal="pig"/>
{:else if display === "bubbles"}
	<IPABubbles ipaObjects={copy.animals.pig} ipaColors={copy.ipaColors.pig} animal="pig"/>
{:else if display === "grid-detail-1"}
{@const ipaAObjects = copy.animals.pig.filter((d) => d?.patterns?.includes("detail-1a"))}
{@const ipaBObjects = copy.animals.pig.filter((d) => d?.patterns?.includes("detail-1b"))}
<div class="two-col small-gap">
	<div style="padding: 2em 0 0 8em"><IPA ipa="oink" lang="trend" colors={copy.ipaColors.pig} audioSrc="pig-english" ipaScale=2.5/></div>
	<div>
		<div class="flex-wrap" style="padding-bottom: 8em">
			{#each ipaAObjects as ipaObj}
			<div class="flex-child third"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.pig} audioSrc={`pig-${ipaObj.lang}`}/></div>
			{/each}
		</div>
		<div class="flex-wrap">
			{#each ipaBObjects as ipaObj}
			<div class="flex-child third"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.pig} audioSrc={`pig-${ipaObj.lang}`}/></div>
			{/each}
		</div>
	</div>
</div>
{:else if display === "grid-detail-2"}
	{@const ipaAObjects = copy.animals.pig.filter((d) => d?.patterns?.includes("detail-2a"))}
	{@const ipaBObjects = copy.animals.pig.filter((d) => d?.patterns?.includes("detail-2b"))}
	{@const ipaCObjects = copy.animals.pig.filter((d) => d?.patterns?.includes("detail-2c"))}
	<!-- <div class="flex-wrap vertical-gap">
		{#each ipaObjects as ipaObj}
		<div class="flex-child fourth"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.pig} audioSrc={`pig-${ipaObj.lang}`}/></div>
		{/each}
	</div> -->
	<div class="flex-wrap" style="padding-bottom: 4em">
		{#each ipaAObjects as ipaObj}
		<IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.pig} audioSrc={`pig-${ipaObj.lang}`}/>
		{/each}
	</div>
	<div class="flex-wrap" style="padding-bottom: 4em">
		{#each ipaBObjects as ipaObj}
		<div class="flex-child third"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.pig} audioSrc={`pig-${ipaObj.lang}`}/></div>
		{/each}
	</div>
	<div class="flex-wrap" style="padding-bottom: 4em">
		{#each ipaCObjects as ipaObj}
		<div class="flex-child third"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.pig} audioSrc={`pig-${ipaObj.lang}`}/></div>
		{/each}
	</div>
{:else if display === "grid-detail-3"}
	{@const ipaObjects = copy.animals.pig.filter((d) => d?.patterns?.includes("detail-3"))}
	<div class="flex-wrap">
		{#each ipaObjects as ipaObj}
		<div class="flex-child"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.pig} audioSrc={`pig-${ipaObj.lang}`}/></div>
		{/each}
	</div>
{/if}
