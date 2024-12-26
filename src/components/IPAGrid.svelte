<script>
  import IPA from "$components/IPA.svelte";
	import volumeIcon from "$svg/volume-icon.svg";
  export let ipaObjects;
  export let ipaColors;
	export let animal;
</script>

<p class="ipa-chart-title">International Phonetic Alphabet (IPA) transcriptions of {animal} sounds in 21 languages</p>
<div class="g-ipa-chart-subtitle"><p class="ipa-chart-subtitle">Click a word to hear it aloud</p>{@html volumeIcon}</div>
<div class="ipa-grid">
	{#each ipaObjects as ipaObj}
	<IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={ipaColors} audioSrc={`${animal}-${ipaObj.lang}`}/>
	{/each}
</div>

<style lang="scss">
	.ipa-grid {
		/**
		* User input values.
		*/
	 	--grid-layout-gap: 0px;
		--grid-column-count: 7;
		--grid-item--min-width: 160px;

		/**
		* Calculated values.
		*/
		--gap-count: calc(var(--grid-column-count) - 1);
		--total-gap-width: calc(var(--gap-count) * var(--grid-layout-gap));
		--grid-item--max-width: calc((100% - var(--total-gap-width)) / var(--grid-column-count));

		display: grid;
		justify-content: center;
		grid-template-columns: repeat(auto-fill, minmax(max(var(--grid-item--min-width), var(--grid-item--max-width)), 1fr));
		grid-gap: var(--grid-layout-gap);
		width: 100vw;
		padding: 1.5em 3em;
	}

	.g-ipa-chart-subtitle {
		display: flex;
		align-items: center;
		gap: 4px;
	}
</style>