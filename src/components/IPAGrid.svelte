<script>
  import IPA from "$components/IPA.svelte";
  export let ipaObjects;
  export let ipaColors;
</script>

<div class="pig">
	{#each ipaObjects as ipaObj}
	<IPA ipa={ipaObj.ipa} word={ipaObj.word} lang={ipaObj.lang} colors={ipaColors} />
	{/each}
</div>

<style lang="scss">
	.pig {
		display: grid;
		justify-content: center;
		grid-template-columns: repeat(7, 1fr);
		width: 100vw;
		padding: 4em;
	}
</style>