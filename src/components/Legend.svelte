<script>
	import { fade } from "svelte/transition";

export let phoneGroups; // object of phone groups for the animal
export let pageColors; // array of colors that are on the slide page

export let activeObj; // object that is currently hovered over

const setActiveObj = (color) => {
  if (activeObj?.color !== color) {
    activeObj = phoneGroups.find((d) => d.color === color);
  } else {
    activeObj = null;
  }
}

</script>

<div class="legend">
  <div class="legend-header">PHONE GROUPS</div>
  <div class="legend-colors">
    {#each phoneGroups as phoneGroup}
    <svg width="20" height="20">
      <g
        on:mouseenter={() => pageColors.includes(phoneGroup.color) && setActiveObj(phoneGroup.color)} 
        on:mouseleave={() => pageColors.includes(phoneGroup.color) && setActiveObj("")}
        on:click={() => pageColors.includes(phoneGroup.color) && setActiveObj(phoneGroup.color)}>
        <circle cx="10" cy="10" r="9" fill="var(--ipa-{phoneGroup.color})" stroke="var(--ipa-{phoneGroup.color}-stroke)" stroke-width="2px" opacity={!pageColors?.includes(phoneGroup.color) && "0.15"}/>
        <circle cx="10" cy="10" r="4" fill="var(--color-gray-800)" opacity={activeObj?.color === phoneGroup.color ? "1" : "0"}/>
      </g> 
    </svg>
    <!-- <div>{JSON.stringify(phoneGroup.color)}</div> -->
    {/each}
    <!-- <div>{phoneGroups}</div> -->
  </div>
  {#key activeObj}
    <div class="legend-description" in:fade={{ delay: 250, duration: 250 }} out:fade={{ duration: 250 }}>
    {#if activeObj}
      <div class="group-name" style="background-color: var(--ipa-{activeObj.color}">{activeObj.groupName}</div>
      <div class="group-def">{activeObj.groupDef}</div>
    {/if}
    </div>
  {/key}
</div>

<style lang="scss">

.legend {
  padding-top: 1em;
  width: 13em;
  // background-color: rgba(255, 255, 255, 0.5);
  // background: 
  //   linear-gradient(to bottom, rgba(255, 255, 255, 1), rgba(255, 255, 255, 0)) 50% 0%,
  //   linear-gradient(to top, rgba(255, 255, 255, 1), rgba(255, 255, 255, 0)) 50% 100%,
  //   linear-gradient(to right, rgba(255, 255, 255, 1), rgba(255, 255, 255, 0)) 0% 50%,
  //   linear-gradient(to left, rgba(255, 255, 255, 1), rgba(255, 255, 255, 0)) 100% 50%;
  // background-size: 100% 50%, 100% 50%, 50% 100%, 50% 100%;
  // background-repeat: no-repeat;
  
}

.legend-colors {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 4px 8px 12px 0;
}

.group-name {
  font-weight: bold;
  width: min-content;
  white-space: nowrap;
  margin-bottom: 4px;
  padding: 0 2px;
}

.legend-header {
  font-size: 13px;
}

.group-def {
  background-color: rgba(255, 255, 255, 1);
}

</style>