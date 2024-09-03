<script>
import world from "./data/world-110m.json";
import * as topojson from "topojson-client";

console.log({world})

let countries = topojson.feature(world, world.objects.countries).features;
let borders = topojson.mesh(world, world.objects.countries, (a, b) => a !== b);


import { geoOrthographic, geoPath } from "d3-geo";

let width = 400;
  $: height = width;

$: projection = geoOrthographic()
    .scale(width / 2)
    .rotate([$xRotation, $yRotation, 0])
    .translate([width / 2, height / 2]);

    $: path = geoPath(projection);


    import Glow from "$components/Glow.svelte";
    import data from "$data/data.json"
    console.log({data})

    // Restructure countries array to include population
  countries.forEach((country) => {
    const metadata = data?.find((d) => d.id === country.id);
    if (metadata) {
      country.population = metadata.population;
      country.country = metadata.country;
    }
  });

    // Color scale
  import { max } from "d3-array";
  import { scaleLinear } from "d3-scale";

  const colorScale = scaleLinear()
    .domain([0, max(data, (d) => d.population)])
    .range(["#26362e", "#0DCC6C"]);

    // Auto rotate the globe, 0.5 degrees per second
  import { timer } from "d3-timer";
  import {spring} from "svelte/motion"

let xRotation = spring(0, {stiffness: 0.08, damping: 0.4});
let yRotation = spring(0, {stiffness: 0.1, damping: 0.7});
const degreesPerFrame = 0.5;

const t = timer(() => {
  if (dragging) return;
  $xRotation += degreesPerFrame;
}, 1);

// Add user interaction
import { onMount } from "svelte";
import { select } from "d3-selection";
import { drag } from "d3-drag";

let globe;
let dragging = false;
const DRAG_SENSITIVITY = 0.5;

onMount(() => {
  const element = select(globe);
  element.call(
    drag()
      .on("drag", (event) => {
        dragging = true;
        $xRotation = $xRotation + event.dx * DRAG_SENSITIVITY;
        $yRotation = $yRotation - event.dy * DRAG_SENSITIVITY;
      })
      .on("end", (event) => {
        dragging = false;
      })
  );
});

let tooltipData;
import Tooltip from "$components/Tooltip.svelte";

</script>

<div class='chart-container' bind:clientWidth={width}>

  <svg {width} {height} bind:this={globe} class:dragging>

     <!-- Filter for drop shadow -->
     <Glow />

    <!-- Globe -->
    <circle 
    r={width / 2} 
    cx={width / 2} 
    cy={height / 2} 
    fill="#1c1c1c"
    filter="url(#glow)" 
  />
  
    <!-- Countries -->
    {#each countries as country}
  <path
    d={path(country)}
    fill={colorScale(country.population || 0)}
    stroke="none"
    on:click={() => (tooltipData = country)}
    on:focus={() => (tooltipData = country)}
    tabIndex="0"
  />
{/each}
  
    <!-- Borders -->
    <path d={path(borders)} fill="none" stroke="#1C1C1C" />

  </svg>

  <Tooltip data={tooltipData} />

</div>

<style>
  .chart-container {
    max-width: 468px;
    margin: auto;
  }

  :global(body) {
    background-color: rgb(40, 40, 40);
  }

  svg {
    overflow: visible;
  }

  .dragging {
  cursor: grabbing;
}

path:focus {
  outline: none;
}

</style>


