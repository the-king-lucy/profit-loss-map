<script>
  import * as d3 from 'd3';
  import { json } from "d3";
  import { geoMercator, geoPath } from "d3-geo";
  import { onMount } from 'svelte';
  import * as topojson from "topojson-client";
  import { scaleLinear } from "d3-scale";
  import { max } from "d3-array";
  import Glow from "./components/Glow.svelte";
  import Tooltip from './components/Tooltip.svelte';
  import Legend from './components/Legend.svelte';

const geojsonData = "/data/gccsa.geojson";
const dataUrl = "/data/domainData.json"; 

let geojsonD;
let dData = [];
let width, height; 
let projection, pathGenerator;
let data = [];
let gccsa = [];
let testMaxValue = 100; 

const valueData = [
  { GCC_CODE21: "1GSYD", value: 95.6 },
  { GCC_CODE21: "1RNSW", value: 95.9 },
  { GCC_CODE21: "2GMEL", value: 97.9 },
  { GCC_CODE21: "2RVIC", value: 98.4 },
  { GCC_CODE21: "3GBRI", value: 99.5 },
  { GCC_CODE21: "3RQLD", value: 97.1 },
  { GCC_CODE21: "4GADE", value: 96.8},
  { GCC_CODE21: "4RSAU", value: 87.1},
  { GCC_CODE21: "5GPER", value: 97.1},
  { GCC_CODE21: "5RWAU", value: 92.6},
  { GCC_CODE21: "6GHOB", value: 96.8},
  { GCC_CODE21: "6RTAS", value: 97.3},
  { GCC_CODE21: "7GDAR", value: 88.4},
  { GCC_CODE21: "7RNTE", value: 92.8},
  { GCC_CODE21: "8ACTE", value: 94},
];

// Set up color scale based on the 'value' column
let colourScale = scaleLinear()
.domain([80, testMaxValue]) 
 .range([ "#ffffff",
 "#0F6CC9"]);

 // Fetch GeoJSON and JSON data
 onMount(async () => {
    // Load GeoJSON
    geojsonD = await json(geojsonData);

    // Dynamically fetch domainData.json
    const response = await fetch(dataUrl);
    dData = await response.json();  // Parse JSON data

    console.log("GeoJSON:", geojsonD);
    console.log("Domain Data:", dData);
});

  $: testMaxValue = d3.max(valueData, d => d.value);

  $: colourScale = scaleLinear()
.domain([80, testMaxValue]) 
 .range([ "#ffffff",
 "#0F6CC9"]);

$: if (geojsonD && width && height) {
    projection = geoMercator()
    .fitExtent([[10, 10], [width -10, height -10]], geojsonD)
  }

  $: if (projection) {
    pathGenerator = geoPath(projection);
  }


 // Map geojson features with JSON data // on:click={() => {hoveredGCCSA = area.thisD}}
 $: if (geojsonD && dData.length > 0 && pathGenerator) {
    gccsa = geojsonD.features.map((feature) => {
      const thisD = dData.find((d) => d.GCC_CODE21 === feature.properties.GCC_CODE21);
      return {
        feature,
        thisD: thisD ? { ...thisD, value: +thisD.value } : null,  // Parse value as a number
        path: pathGenerator(feature)
      };
    });
  }
  //});


 let hoveredGCCSA;
 let tooltipTop = 0;
  let tooltipLeft = 0;
  let tooltipPosition;
 

 function handleMouseMove(e, area) {
    hoveredGCCSA = area.thisD;
    tooltipTop = e.clientY + 10; // Position the tooltip slightly below the cursor
    tooltipLeft = e.clientX + 10;
  }




</script>

<div class="chart-container" bind:clientWidth={width} bind:clientHeight={height}>
 
  <div class="header">
    <h1>Which area made the most profit on house resales?</h1>
  <h2>Hover over your area to find out the average profit or loss on the resale of a house.</h2>
  </div>
  
  

<svg class="map" viewBox={`0 0 ${width} ${height}`} preserveAspectRatio="none">

  <Glow />

  

  {#each gccsa as area}
  <!-- svelte-ignore ally-click-events-have-key-events -->
  <path
    d={area.path}
        stroke={"#ffffff"}
        stroke-linecap={"round"}
        stroke-linejoin={"round"}
        filter="url('#glow')"
        fill={area.thisD ? colourScale(area.thisD.value) : '#ccc'}  
        class:active={hoveredGCCSA === area.thisD}
        on:mouseenter={() => hoveredGCCSA = area.thisD}
      on:mousemove={(e) => handleMouseMove(e, area)}
      on:mouseleave={() => { hoveredGCCSA = null }}
  />
{/each}



</svg>
<Legend />

<h3 class="footer">Source: <a href="https://www.domain.com.au/research/profit-and-loss-report-1312028/" target="_blank">Profit and Loss Report by Domain Research</a></h3>
  
{#if hoveredGCCSA}
    <Tooltip dData={hoveredGCCSA} top={tooltipTop} left={tooltipLeft}  />
  {/if}

  


</div>

<style>

.chart-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  overflow: hidden;
  align-items: center;
  width: 100vw;
  height: 100vh;
  }

  .map-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.map {
  width: 100%;
  height: auto;  /* Automatically adjust height based on width */
}


  path {
  transition: fill 0.3s ease; /* Fade duration of 0.3 seconds */
}

  path.active {
fill: black;
backdrop-filter: blur(8px);
  }

h1, h2 {
  color: black;
  text-align: left;
}

h1 {
  padding-top:0.5rem;
  font-size: 1.75rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

h2 {
  padding-top: 0.1rem;
  font-size: 1.25rem;
  font-weight: 200;
  margin-bottom: 1rem;
}

.footer {
  text-align: left;
}

.header .footer {
  align-self: flex-start
}
 

</style>

