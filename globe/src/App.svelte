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
let geojson;
let dData = [];
let width, height; // Set default width and height
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
.domain([0, testMaxValue]) 
 .range([ "#CCE8FA",
 "#0F6CC9"]);

// Fetch GeoJSON and CSV data
//json(geojsonData).then((data) => geojsonD = data);
//$: console.log(geojsonD);

// d3.csv('/data/domainData.csv').then(function(data) {dData = data; console.log({dData});});
//d3.csv('/data/domainData.csv').then(function(data) {dData = data; console.log({dData});});

//import dData from "/data/domainData.json";

 // Map projection and path generator

 // Fetch GeoJSON and JSON data
 onMount(async () => {
    // Load GeoJSON
    geojsonD = await json(geojsonData);

    // Dynamically fetch domainData.json
    const response = await fetch(dataUrl);
    dData = await response.json();  // Parse JSON data

    console.log("GeoJSON:", geojsonD);
    console.log("Domain Data:", dData);

    dData.forEach(d => {
  const val = parseFloat(d.value);  // Convert to number if it's a string
  console.log(`GCC_CODE21: ${d.GCC_CODE21}, Value: ${d.value}, Parsed Value: ${val}, Is NaN: ${isNaN(val)}`);
});



const testMaxValue = d3.max(valueData, d => d.value);
  console.log("Test Max Value:", testMaxValue);


 // Update color scale domain after calculating max value
 $: colourScale = scaleLinear()
      .domain([80, testMaxValue])
      .range(["#ffffff", "#0F6CC9"]);

      //#CCE8FA", "#0F6CC9"
    //dData.forEach(d => {
  //console.log(Object.keys(d));  // Log the keys of each object in dData
//});

    // Process projection and path only after both GeoJSON and dData are loaded
    if (geojsonD && dData.length > 0) {
 $: projection = geoMercator().fitSize([width, height], geojsonD)
 .center(d3.geoCentroid(geojsonD))  // Center it based on the geoJSON data
        .translate([width / 2, height / 2]);  // Center the projection in the SVG;
 $: console.log(projection);
 $: pathGenerator = geoPath(projection);
 $: console.log(pathGenerator);

 // Map geojson features with JSON data // on:click={() => {hoveredGCCSA = area.thisD}}
 gccsa = geojsonD.features.map((feature) => {
        const thisD = dData.find((d) => d.GCC_CODE21 === feature.properties.GCC_CODE21);
        return {
          feature,
          thisD: thisD ? { ...thisD, value: +thisD.value } : null,  // Parse value as a number
          path: pathGenerator(feature)
        };
      });
      console.log("Mapped GCCSA:", gccsa);
    }
  });

 $: console.log("Color Scale Domain:", colourScale.domain());
 $: console.log("Color Scale Range:", colourScale.range());

 let hoveredGCCSA;
 let tooltipTop = 0;
  let tooltipLeft = 0;
  let tooltipPosition;
 

 $: console.log({hoveredGCCSA});

 function handleMouseMove(e, area) {
    hoveredGCCSA = area.thisD;
    tooltipTop = e.clientY + 10; // Position the tooltip slightly below the cursor
    tooltipLeft = e.clientX + 10;
  }

  console.log({Legend})

</script>

<div class="chart-container" bind:clientWidth={width} bind:clientHeight={height}>
  <Legend />
<svg class="map" viewBox="0 0 {width} {height}" preserveAspectRatio="xMidYMid meet">

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

{#if hoveredGCCSA}
    <Tooltip dData={hoveredGCCSA} top={tooltipTop} left={tooltipLeft}  />
  {/if}

  


</div>

<style>

.chart-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  width: 100vw;
  height: 100vh;
  }


  path {
  transition: fill 0.3s ease; /* Fade duration of 0.3 seconds */
}

  path.active {
fill: #ffffff;
opacity: 0.5;
  }


 

</style>

