<script>
  export let ipa;
  export let word;
  export let lang;
  export let colors;

  let letters = ipa.split(/(?![ːɲ])(?=\S)/);
</script>

<div class="ono-container">
  <div class="word {lang}">{word}</div>
  <div class="ipa-container">
    <div>[</div>
    {#each letters as letter}
      <div class="ipa-letter {colors[letter]}">
        {letter}
      </div>
    {/each}
    <div>]</div>
  </div>
  <div class="lang">{lang.toUpperCase()}</div>
</div>

<style lang="scss">
  .ono-container {
    display: grid;
    justify-items: center;
    gap: 0.4em;
    margin: 2em;
  }
  .ipa-container {
    display: flex;
    align-items: center;
    gap: 1.5px;
    font-family: var(--font-ipa);
    font-weight: bold;
    font-size: 2em;
  }
  .ipa-letter {
    height: 1.15em;
    line-height: 1.05;
    &.pink {
      background-color: var(--ipa-pink);
    }
    &.orange {
      background-color: var(--ipa-orange);
    }
    &.yellow {
      background-color: var(--ipa-yellow);
    }
    &.green {
      background-color: var(--ipa-green);
    }
    &.blue {
      background-color: var(--ipa-blue);
    }
    &.purple {
      background-color: var(--ipa-purple);
    }
    &.pink75-yellow25 {
      background: linear-gradient(to right, var(--ipa-pink) 78%, var(--ipa-yellow) 22%);
    }
  }
  .word {
    &.hindi {
      font-family: "IBM Plex Sans Devanagari";
    }
  }
  .lang {
    color: var(--color-gray-500);
;  }
</style>
