<script>
  export let transcription;

  let letters = [...transcription];
  // letters.unshift('[');
  // letters.push(']');
</script>

<div class="ipa-container">
  <div>[</div>
  {#each letters as letter}
    <div class="ipa-letter">
      {letter}
    </div>
  {/each}
  <div>]</div>
</div>

<style lang="scss">
  .ipa-container {
    display: flex;
    gap: 2px;
  }
  .ipa-letter {
    background-color: red;
  }
</style>
