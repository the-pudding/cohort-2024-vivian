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
		<img src={`assets/duck-face-logo.png`} width=125 height=125 alt={`duck face doodle`}/>
		<p class="scrolly-cover-hed">Duck</p>
	</div>
{:else if display === "grid"}
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
	{@const ipaObjectsA = copy.animals.duck.filter((d) => d?.patterns?.includes("detail-3a"))[0]}
	{@const ipaObjectsB = copy.animals.duck.filter((d) => d?.patterns?.includes("detail-3b"))}
	{@const ipaObjectsC = copy.animals.duck.filter((d) => d?.patterns?.includes("detail-3c"))}
	<div class="two-col smaller-gap">
		<div style="padding: 2em 0 0 5em"><IPA ipa="kwak2" lang="english + 3 others" colors={copy.ipaColors.duck} audioSrc="duck-english" ipaScale=2.5/></div>
		<div>
			<IPA ipa={ipaObjectsA.ipa} word={ipaObjectsA.word} lang={ipaObjectsA.lang} colors={copy.ipaColors.duck} audioSrc={`duck-${ipaObjectsA.lang}`} ipaScale=1.25/>
			<div class="flex-wrap">
				{#each ipaObjectsB as ipaObj}
				<div class="flex-child fourth"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.duck} audioSrc={`duck-${ipaObj.lang}`} ipaScale=1.25/></div>
				{/each}
			</div>
			<div class="flex-wrap">
				{#each ipaObjectsC as ipaObj}
				<div class="flex-child fourth"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.duck} audioSrc={`duck-${ipaObj.lang}`} ipaScale=1.25/></div>
				{/each}
			</div>
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
	{:else if display === "grid-detail-4.5"}
	{@const ipaDuckObjects = copy.animals.duck.filter((d) => d?.patterns?.includes("detail-4"))}
	{@const ipaCatObjects = copy.animals.cat.filter((d) => d?.patterns?.includes("detail-3"))}
	<div class="two-col small-gap">
		<div style="padding: 2em 0 0 8em"><IPA ipa="kwak2" lang="english + 3 others" colors={copy.ipaColors.duck} audioSrc="duck-english" ipaScale=2.5/></div>
		<div class="flex-wrap">
			{#each ipaDuckObjects as ipaObj}
			<div class="flex-child third"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.duck} audioSrc={`duck-${ipaObj.lang}`}/></div>
			{/each}
		</div>
	</div>
	<div class="two-col small-gap" style="margin-top: 8em">
		<div style="padding: 2em 0 0 4em"><IPA ipa="miau" lang="english + 14 others" colors={copy.ipaColors.cat} audioSrc="cat-english" ipaScale=2.5/></div>
		<div class="flex-wrap">
			{#each ipaCatObjects as ipaObj}
			<div class="flex-child third"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.cat} audioSrc={`cat-${ipaObj.lang}`}/></div>
			{/each}
		</div>
	</div>
{:else if display === "grid-detail-5"}
	{@const ipaObjects = copy.animals.duck.filter((d) => d?.patterns?.includes("detail-5"))}
	<div class="two-col small-gap">
		<div style="padding: 2em 0 0 5em"><IPA ipa="kwak2" lang="english + 3 others" colors={copy.ipaColors.duck} audioSrc="duck-english" ipaScale=2.5/></div>
		<div class="flex-wrap">
			{#each ipaObjects as ipaObj}
			<div class="flex-child"><IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={copy.ipaColors.duck} audioSrc={`duck-${ipaObj.lang}`} ipaScale=1.25/></div>
			{/each}
		</div>
	</div>

	<!-- czech, ukrainian, russian -->
{/if}
