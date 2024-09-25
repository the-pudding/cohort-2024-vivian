<script>
	import { getContext } from "svelte";
  import IPA from "$components/IPA.svelte";
	const copy = getContext("copy");
</script>

{#each copy.animals.cat as ono}
<IPA ipa={ono.ipa} word={ono.word} lang={ono.lang} colors={copy.ipaColors.cat} />
{/each}