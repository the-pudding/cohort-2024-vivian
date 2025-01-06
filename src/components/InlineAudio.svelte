<script>
	import volumeIcon from "$svg/volume-icon.svg";

  export let audioSrc;
  export let marginRight = false;

  function playIPAAudio(audioSrc) {
    const audio = new Audio(audioSrc);
    audio.play();
  }
</script>

<div class="inline-audio {marginRight && "margin-right"}" on:click={() => playIPAAudio(`audio/${audioSrc}.mp3`)}>
  <span>
    <slot />
  </span>
  <div class="volume-icon">{@html volumeIcon}</div>
</div>

<style lang="scss">
  .inline-audio {
    display: inline-flex;
    cursor: pointer;
    gap: 2px;
    align-items: center;
    margin-left: 4px;

    &.margin-right {
      margin-right: 4px;
    }
  }

  .volume-icon {
    height: 22px;
  }
</style>