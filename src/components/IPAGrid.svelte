<script>
  import IPA from "$components/IPA.svelte";
  export let ipaObjects;
  export let ipaColors;
</script>

<div class="ipa-grid">
	{#each ipaObjects as ipaObj}
	<IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={ipaColors} />
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
		padding: 3em;
	}
</style>