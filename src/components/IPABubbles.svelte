<script>
  import { onMount } from "svelte";
  import { select, forceSimulation, forceX, forceY, forceCollide } from "d3";
	import { blur } from 'svelte/transition';

  export let ipaObjects;
  export let ipaColors;
  export let animal

  let bubbleData = [];

  for (const ipaObj of ipaObjects) {
    const letters = ipaObj.ipa.split(/(?<![ˈ͡])(?![̌ːɲ\d])/);

    for (const letter of letters) {
      let letterColor = ipaColors[letter];

      if (letterColor) {
        if (!bubbleData.find((d) => d.color === letterColor)) {
          // create an object for this color
          let sumLetter = Object.entries(ipaColors).find(([key, value]) => value === letterColor)?.[0];
          if (sumLetter.includes("2")) {
            sumLetter = sumLetter.slice(0, -1);
          } else if (sumLetter === "ʲ") {
            sumLetter = "y";
          }
          if (letterColor.includes("pink") && letterColor.includes("yellow")) {
            bubbleData.find((d) => d.color === "pink").count++;
            bubbleData.find((d) => d.color === "yellow").count++;
          } else {
            bubbleData.push({ sumLetter: sumLetter, color: letterColor, count: 1 })
          }
        } else {
            bubbleData.find((d) => d.color === letterColor).count++;
        }
      }
    }
  }

  let svg;
  let width;
  let height;
  let container;
  let bbox;

  $: {
    if (svg) {
      // Adjust the SVG size
      select(svg).attr("width", width).attr("height", height);

      // Reconfigure simulation forces on resize
      simulation.force("x", forceX(width / 2).strength(0.05));
      simulation.force("y", forceY(height / 2).strength(0.08));
      simulation.alpha(1).restart();
    }
  }

  let simulation;
  
  onMount(() => {
    simulation = forceSimulation(bubbleData)
      .force("x", forceX(width / 2).strength(0.05))
      .force("y", forceY(height / 2).strength(0.08))
      .force("collision", forceCollide(d => d.count * 10 + 12))
      .on("tick", ticked);

    const svgElement = select(svg)
      .attr("width", width)
      .attr("height", height);

    const graphGroup = svgElement.append("g");

    const node = graphGroup.selectAll("g")
      .data(bubbleData)
      .enter()
      .append("g")
      .attr("class", "node");

    node.append("circle")
      .attr("r", d => d.count * 9 + 2) // Scale bubble size
      .attr("fill", d => `var(--ipa-${d.color})`)
      .attr("stroke", d => `var(--ipa-${d.color}-stroke)`)
      .attr("stroke-width", d => `${d.count * 0.05 + 1}px`);

    node.append("text")
      .text(d => d.sumLetter)
      .attr("text-anchor", "middle")
      .attr("dominant-baseline", "middle")
      .attr("font-size", d => `${d.count * 6 + 6}px`)
      .attr("font-family", "var(--font-ipa)")
      .attr("font-weight", "bold");

    node.append("text")
      .html(d => d.count < 6 ? `<tspan>${d.count}</tspan>` : `<tspan>${d.count}</tspan> LANGUAGES`)
      .attr("y", d => d.count * 9.5 + 14)
      .attr("text-anchor", "middle")
      .attr("font-size", "12px")
      .attr("fill", "var(--color-gray-700)");

    function ticked() {
      node.attr("transform", d => `translate(${d.x}, ${d.y})`);
    }; 

    setTimeout(() => {
      bbox = graphGroup.node().getBBox(); // Get bounding box of the graph group
    }, 1000);
  });

</script>


<div 
  bind:clientWidth={width}
  bind:clientHeight={height}
  bind:this={container}
  class="ipa-bubbles">
  <svg bind:this={svg}>
    {#if bbox}
    <text class="ipa-chart-title" x="50%" y={bbox.y - 30} text-anchor="middle" transition:blur>Number of phone group occurences in {animal} sounds across 21 languages</text>
    {/if}
  </svg>
</div>

<style lang="scss">
  .ipa-bubbles {
    width: 100%; 
    height: 100vh; 
    overflow: hidden;
  }
  svg {
    display: block;
    margin: 0 auto;
  }

  :global(tspan) {
    font-weight: bold;
  }
</style>