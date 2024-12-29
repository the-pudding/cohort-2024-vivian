<script>
	import { getContext } from "svelte";
	const copy = getContext("copy");
	import IPAGrid from "./IPAGrid.svelte";
	import IPABubbles from "./IPABubbles.svelte";
	import IPA from "$components/IPA.svelte";

	export let display = "grid"; // "grid" | "bubbles"
</script>

{#if display === "grid"}
	<IPAGrid ipaObjects={copy.animals.duck} ipaColors={copy.ipaColors.duck} animal="duck"/>
{:else if display === "bubbles"}
	<IPABubbles ipaObjects={copy.animals.duck} ipaColors={copy.ipaColors.duck} animal="duck"/>
{:else if display === "grid-detail-1"}
	{@const ipaObjects = copy.animals.duck.filter((d) => d?.patterns?.includes("trend"))}
	<div class="two-col small-gap">
		<div style="padding: 2em 0 0 8em"><IPA ipa="kwak2" lang="trend" colors={copy.ipaColors.duck} audioSrc="duck-english" ipaScale=2.5/></div>
		<div class="flex-wrap">
			{#each ipaObjects as ipaObj}
			<div class="flex-child"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.duck} audioSrc={`duck-${ipaObj.lang}`}/></div>
			{/each}
		</div>
	</div>
{:else if display === "grid-detail-2"}
	{@const ipaObjects = copy.animals.duck.filter((d) => d?.patterns?.includes("detail-2"))}
	<div class="two-col small-gap">
		<div style="padding: 2em 0 0 8em"><IPA ipa="kwak2" lang="english + 3 others" colors={copy.ipaColors.duck} audioSrc="duck-english" ipaScale=2.5/></div>
		<div class="flex-wrap">
			{#each ipaObjects as ipaObj}
			<div class="flex-child third"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.duck} audioSrc={`duck-${ipaObj.lang}`} ipaScale=1.25/></div>
			{/each}
		</div>
	</div>
{:else if display === "grid-detail-3"}
	{@const ipaObjects = copy.animals.duck.filter((d) => d?.patterns?.includes("detail-3"))}
	<div class="two-col smaller-gap">
		<div style="padding: 2em 0 0 5em"><IPA ipa="kwak2" lang="english + 3 others" colors={copy.ipaColors.duck} audioSrc="duck-english" ipaScale=2.5/></div>
		<div class="flex-wrap">
			{#each ipaObjects as ipaObj}
			<div class="flex-child"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.duck} audioSrc={`duck-${ipaObj.lang}`} ipaScale=1.25/></div>
			{/each}
		</div>
	</div>
{:else if display === "grid-detail-4"}
	{@const ipaObjects = copy.animals.duck.filter((d) => d?.patterns?.includes("detail-4"))}
	<div class="two-col small-gap">
		<div style="padding: 2em 0 0 8em"><IPA ipa="kwak2" lang="english + 3 others" colors={copy.ipaColors.duck} audioSrc="duck-english" ipaScale=2.5/></div>
		<div class="flex-wrap">
			{#each ipaObjects as ipaObj}
			<div class="flex-child"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.duck} audioSrc={`duck-${ipaObj.lang}`}/></div>
			{/each}
		</div>
	</div>
{:else if display === "grid-detail-5"}
	{@const ipaObjects = copy.animals.duck.filter((d) => d?.patterns?.includes("detail-5"))}
	<div class="two-col small-gap">
		<div style="padding: 2em 0 0 8em"><IPA ipa="kwak2" lang="english + 3 others" colors={copy.ipaColors.duck} audioSrc="duck-english" ipaScale=2.5/></div>
		<div class="flex-wrap">
			{#each ipaObjects as ipaObj}
			<div class="flex-child"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.duck} audioSrc={`duck-${ipaObj.lang}`}/></div>
			{/each}
		</div>
	</div>

	<!-- czech, ukrainian, russian -->
{/if}
