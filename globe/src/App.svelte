<script>
  import * as d3 from 'd3';
  import { json } from "d3";
  import { geoMercator, geoPath } from "d3-geo";
  import { onMount } from 'svelte';
  import * as topojson from "topojson-client";
  import { scaleLinear } from "d3-scale";
  import { max } from "d3-array";

const geojsonData = "/data/gccsa.geojson";
const dataUrl = "/data/domainData.json"; 

let geojsonD;
let geojson;
let dData = [];
let width, height;
let aspectRatio;
let projection, pathGenerator;
let data = [];
let gccsa = [];

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

    //dData.forEach(d => {
  //console.log(Object.keys(d));  // Log the keys of each object in dData
//});

    // Process projection and path only after both GeoJSON and dData are loaded
    if (geojsonD && dData.length > 0) {
 $: projection = geoMercator().fitSize([width, height], geojsonD);
 $: console.log(projection);
 $: pathGenerator = geoPath(projection);
 $: console.log(pathGenerator);

 // Map geojson features with JSON data
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

 
    

  const maxValue = d3.max(dData, d => d.value) || 100;  // Should give approximately 100
console.log("Max Value:", maxValue); 

// Set up color scale based on the 'value' column
const colourScale = scaleLinear()
.domain([0, maxValue]) 
 .range([ "#CCE8FA",
 "#0F6CC9"]);


 $: console.log("Color Scale Domain:", colourScale.domain());
 $: console.log("Color Scale Range:", colourScale.range());

 let hoveredGCCSA = null;

 $: console.log(hoveredGCCSA);

</script>

<div class="chart-container" bind:clientWidth={width} bind:clientHeight={height}>
<svg width={width} height={height}>

  {#each gccsa as area}
  <path
    d={area.path}
        stroke={"#8c8c8c"}
        stroke-linecap={"round"}
        stroke-linejoin={"round"}
        fill={colourScale(area.value || 0)}
        class:active={hoveredGCCSA === area.feature.properties.GCC_NAME21}
        on:mouseenter={() => hoveredGCCSA = area.feature.properties.GCC_NAME21}
  />
{/each}

</svg>

</div>

<style>

  div {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
  }

  path.active {
fill: #ffffff;
  }
 

</style>

