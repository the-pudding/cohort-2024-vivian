<script>
  import IPA from "$components/IPA.svelte";
	import { getContext } from "svelte";
  
	export let animal;
  export let hoverGroupColor = undefined;

	const copy = getContext("copy");
</script>

<div class="ipa-column">
	{#each copy.animals[animal] as ipaObj}
	<IPA ipa={ipaObj.ipa} lang={ipaObj.lang} colors={copy.ipaColors[animal]} audioSrc={`${animal}-${ipaObj.lang}`} ipaColumnDisplay={true} ipaScale={0.95} {hoverGroupColor}/>
	{/each}
</div>

<style lang="scss">
	.ipa-column {
    margin: 2em 0 0 2em;
    transform: scale(0.75);
    transform-origin: left;
    @media screen and (max-width: 1100px) and (max-height: 750px) {
      display: none;
    }

    @media screen and (max-width: 1500px) {
      transform: scale(0.7);
    }
	}
</style>