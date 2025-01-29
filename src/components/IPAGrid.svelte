<script>
  import IPA from "$components/IPA.svelte";
  export let ipaObjects;
  export let ipaColors;
	export let animal;
</script>

<p class="ipa-chart-title">International Phonetic Alphabet (IPA) transcriptions of {animal} sounds in 21 languages</p>
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

		@media screen and (max-width: 600px) {
			--grid-item--min-width: 125px;
			padding: 1em;
			transform: scale(0.9);
		}
		@media screen and (max-width: 415px) {
			--grid-item--min-width: 110px;
			transform: scale(0.85);
		}

		@media screen and (max-width: 370px) {
			--grid-item--min-width: 95px;
		}
	}

	@media screen and (max-width: 600px) {
		.ipa-chart-title {
			display: none;
		}
	}

</style>