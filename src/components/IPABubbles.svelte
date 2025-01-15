<script>
  import { onMount } from "svelte";
  import { select, forceSimulation, forceX, forceY, forceCollide } from "d3";
	import { blur } from 'svelte/transition';
	import IPAColumn from "$components/IPAColumn.svelte";

  export let ipaObjects;
  export let ipaColors;
  export let animal;

  let bubbleData = [];

  const playIPAAudio = (audioSrc) => {
    console.log("clicked");
    const audio = new Audio(audioSrc);
    audio.play();
  }

  for (const ipaObj of ipaObjects) {
    let letters = ipaObj.ipa.split(/(?<![ˈ͡])(?![̌ːɲʲ\d])/);
    // letters = letters.filter((item, index) => letters.indexOf(item) === index);
    let lettersCounted = [];

    for (const letter of letters) {
      let letterColor = ipaColors[letter];
      let sumLetter = Object.entries(ipaColors).find(([key, value]) => value === letterColor)?.[0];

      if (letterColor) {
        if (!bubbleData.find((d) => d.color === letterColor)) {
          // create an object for this color
          if (sumLetter.includes("2")) {
            sumLetter = sumLetter.slice(0, -1);
          } 
          if (animal === "duck" && sumLetter === "t") {
            sumLetter = "tʃ";
          }
          if (letterColor.includes("pink") && letterColor.includes("yellow")) {
            bubbleData.find((d) => d.color === "pink").count++;
            bubbleData.find((d) => d.color === "yellow").count++;
          } else if (letterColor.includes("green") && letterColor.includes("yellow")) {
            bubbleData.find((d) => d.color === "green").count++;
            bubbleData.find((d) => d.color === "yellow").count++;
          } else {
            bubbleData.push({ sumLetter: sumLetter, color: letterColor, count: 1 })
          }
          lettersCounted.push(sumLetter);
        } else {
          if (letter === "͡ʃ" || lettersCounted.includes(sumLetter)) { continue; }
          bubbleData.find((d) => d.color === letterColor).count++;
          lettersCounted.push(sumLetter);
        }
      }
    }
  }

  let svg;
  let width;
  let height;
  let scaleFactor;
  let container;
  let bbox;

  const aspectRatio = width > 1500 ? 0.6 : 0.85;
    
  // Add a scaling function for bubble sizes
  const getScaleFactor = (width)  => {
    // Base scale for a "standard" width of 1000px
    const baseWidth = 1100;
    const scale = width / baseWidth;
    // Clamp the scale between 0.5 and 1.5 to prevent extremes
    return Math.min(Math.max(scale, 0.5), 1.5);
  }

  // Responsive sizing function
  const updateSize = () => {
    width = container.clientWidth;
    // Set height based on aspect ratio, with a minimum height
    height = Math.max(width * aspectRatio, 100);
    scaleFactor = getScaleFactor(width);
  }

  let simulation;

  // Watch for container size changes
  $: {
    if (container) {
      updateSize();
    }
  }

  onMount(() => {

    // Initial size setup
    updateSize();

    simulation = forceSimulation(bubbleData)
      .force("x", forceX(width / 2).strength(width < 800 ? 0.08 : 0.05))
      .force("y", forceY(height / 2).strength(width < 800 ? 0.05 : 0.08))
      .force("collision", forceCollide(d => (d.count * 9 + 12) * scaleFactor))
      .on("tick", ticked);

    const svgElement = select(svg)
      .attr("viewBox", `0 0 ${width} ${height}`)
      .attr("preserveAspectRatio", "xMidYMid meet");

    const graphGroup = svgElement.append("g");

    const node = graphGroup.selectAll("g")
      .data(bubbleData)
      .enter()
      .append("g")
      .attr("class", "node")
      .attr("cursor", "pointer")
      .on("click", (event, d) => {playIPAAudio(`audio/ipa-${d.sumLetter}.mp3`);})

    node.append("circle")
      .attr("r", d => (d.count * 8 + 2) * scaleFactor) // Scale bubble size
      .attr("fill", d => `var(--ipa-${d.color})`)
      .attr("stroke", d => `var(--ipa-${d.color}-stroke)`)
      .attr("stroke-width", d => `${(d.count * 0.05 + 1) * scaleFactor}px`);

    node.append("text")
      .text(d => d.sumLetter)
      .attr("class", "ipa-letter")
      .attr("text-anchor", "middle")
      .attr("dominant-baseline", "middle")
      .attr("font-size", d => `${(d.count * 6 + 6) * scaleFactor}px`)
      .attr("font-family", "var(--font-ipa)")
      .attr("font-weight", "bold");

    // volume icon
    fetch('/assets/volume-icon.svg')
      .then(response => response.text())
      .then(svgContent => {
        node.filter(d => d.count > 4)
          .append("g")
          .attr("class", "icon")
          .attr("stroke", d => `var(--ipa-${d.color}-stroke)`)
          .attr("transform", d => `translate(0, ${1.75 * d.count * getScaleFactor(width) + 20})`)
          .html(svgContent)
          .each(function() {
            const icon = select(this).select("svg");
            icon
              .attr("width", 16 * getScaleFactor(width))
              .attr("height", 16 * getScaleFactor(width))
              .attr("x", -8 * getScaleFactor(width))
              .attr("y", -8 * getScaleFactor(width));
          });
      });


    node.append("text")
      .html(d => d.count < 6 || width < 800 ? `<tspan>${d.count}</tspan>` : `<tspan>${d.count}</tspan> LANGUAGES`)
      .attr("class", "count-text")
      .attr("y", d => (d.count * 8.5 + 16) * scaleFactor + 4)
      .attr("text-anchor", "middle")
      .attr("font-size", `${Math.max(13 * scaleFactor, 12)}px`)
      .attr("fill", "var(--color-gray-700)");

    function ticked() {
      node.attr("transform", d => `translate(${d.x}, ${d.y})`);
    }; 

    setTimeout(() => {
      bbox = graphGroup.node().getBBox(); // Get bounding box of the graph group
    }, 1000);
  });

  $: {
    if (svg && width && height & simulation) {
      select(svg)
        .attr("viewBox", `0 0 ${width} ${height}`)
        .attr("preserveAspectRatio", "xMidYMid meet");

      simulation
        .force("x", forceX(width / 2).strength(0.05))
        .force("y", forceY(height / 2).strength(0.08))
        .force("collision", forceCollide(d => (d.count * 9 + 12) * scaleFactor))
        .alpha(1)
        .restart();

      // Update all scaled elements
      select(svg).selectAll("circle")
        .attr("r", d => (d.count * 8 + 2) * scaleFactor)
        .attr("stroke-width", d => `${(d.count * 0.05 + 1) * scaleFactor}px`);

      select(svg).selectAll(".ipa-letter")
        .attr("font-size", d => `${(d.count * 6 + 6) * scaleFactor}px`);

      select(svg).selectAll(".count-text")
        .attr("y", d => (d.count * 8.5 + 16) * 1.5 * scaleFactor);

    }
  }

</script>


<div 
  bind:clientWidth={width}
  bind:clientHeight={height}
  bind:this={container}
  class="ipa-bubbles">
  <div class="ipa-column-container">
    <IPAColumn {animal} />
  </div>
  <svg bind:this={svg}>
    {#if bbox}
    <foreignObject 
      width={width} 
      height=100 
      x="0%" 
      y={width < 600 ? bbox.y - 100 : (width < 800 ? bbox.y - 70 : bbox.y - 50 * scaleFactor)}>
      <div 
        class="ipa-chart-title" 
        text-anchor="middle" 
        style={`font-size: ${Math.max(20 * scaleFactor, 16)}px`} 
        transition:blur>
        Number of phone group occurences in {animal} sounds across 21 languages
      </div>
    </foreignObject>
    {/if}
  </svg>
</div>

<style lang="scss">
  .ipa-bubbles {
    width: 100%;
    height: 100%;
    min-height: 400px;
    position: relative;
		margin-top: 20px;
    overflow: hidden;
    display: flex;
  }
  svg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  :global(tspan) {
    font-weight: bold;
  }
  

  .ipa-chart-title {
    text-align: center;
    @media screen and (max-width: 768px) {
      margin: 2em;
      max-width: 100%;
    }
  }

  .ipa-column-container {
    display: flex;
    align-items: center;
    z-index: 10;
  }
</style>