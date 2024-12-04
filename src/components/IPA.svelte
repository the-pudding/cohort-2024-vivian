<script>
  export let ipa;
  export let word;
  export let lang;
  export let colors;
  export let animal;

  let letters = ipa.split(/(?<![ˈ͡])(?![̌ːɲ\d])/);

  function playIPAAudio(audioSrc) {
    const audio = new Audio(audioSrc);
    audio.play();
  }

</script>

<div class="ono-container" on:click={() => playIPAAudio(`audio/${animal}-${lang}.mp3`)}>
  <div class="word {lang}">{word}</div>
  <div class="ipa-container">
    <div>[</div>
    {#each letters as letter}
      <div class="ipa-letter {colors[letter]}">
        {letter.replace(/[0-9]/g, '')}
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
    margin: 1.25em;
    cursor: pointer;
  }
  .ipa-container {
    display: flex;
    align-items: center;
    gap: 1.5px;
    font-family: var(--font-ipa);
    font-weight: bold;
    font-size: 28px;
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
    &.indigo {
      background-color: var(--ipa-indigo);
    }
    &.purple {
      background-color: var(--ipa-purple);
    }
    &.brown {
      background-color: var(--ipa-brown);
    }
    &.seafoam {
      background-color: var(--ipa-seafoam);
    }
    &.stone {
      background-color: var(--ipa-stone);
    }
    &.pink-50 {
      background-color: var(--ipa-pink-50);
    }
    &.purple-50 {
      background-color: var(--ipa-purple-50);
    }
    &.indigo-50 {
      background-color: var(--ipa-indigo-50);
    }
    &.pink75-yellow25 {
      background: linear-gradient(to right, var(--ipa-pink) 78%, var(--ipa-yellow) 22%);
    }
    &.pink50-yellow50 {
      background: linear-gradient(to right, var(--ipa-pink) 50%, var(--ipa-yellow) 50%);
    }
  }
  .word {
    &.hindi {
      font-family: "IBM Plex Sans Devanagari";
    }
    font-size: 14px;
  }
  .lang {
    color: var(--color-gray-500);
    font-size: 14px;
;  }
</style>
