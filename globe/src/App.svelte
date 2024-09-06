<script>
  import * as d3 from 'd3';
  import { json } from "d3";
  import { geoMercator, geoPath } from "d3-geo";
  import { onMount } from 'svelte';
  import * as topojson from "topojson-client";
  import { scaleLinear } from "d3-scale";
  import { max } from "d3-array";

const geojsonData = "/data/gccsa.geojson";

let geojsonD;
let geojson;
let width, height;
let aspectRatio;
let projection;
let dData = [];

json(geojsonData).then((data) => geojsonD = data);

$: console.log(geojsonD);

d3.csv('/data/domainData.csv').then(function(data) {dData = data; console.log("Domain Data Loaded:", dData);});


 $: projection = geoMercator().fitSize([width, height], geojsonD);
 $: console.log(projection);
 $: pathGenerator = geoPath(projection);
 $: console.log(pathGenerator);

 let gccsa = [];
 $: if (geojsonD) gccsa = geojsonD.features.map(feature => { return {feature, path: pathGenerator(feature)}});

 $: console.log(gccsa);

 let hoveredGCCSA = null;

 $: console.log(hoveredGCCSA);

 const colourScale = scaleLinear()
 .domain(0, max(dData, (d) => d.value))
 .range([ "#CCE8FA",
 "#0F6CC9"])


</script>

<div class="chart-container" bind:clientWidth={width} bind:clientHeight={height}>

  
<svg width={width} height={height}>

  {#each gccsa as area}
  <path
    d={area.path}
        stroke={"#8c8c8c"}
        stroke-linecap={"round"}
        stroke-linejoin={"round"}
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

