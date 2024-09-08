<script>
    export let dData;
    export let top;
  export let left;

    console.log("Hello from tooltip", dData)

    import { format } from "d3-format";
  const suffixFormat = (d) => format(".2~s")(d).replace(/G/, "B")

  import {fade} from "svelte/transition";
  </script>
  
  {#if dData}
    <div in:fade class="tooltip" style="top: {top}px; left: {left}px;">
       <!-- Conditionally render based on gcc_code -->
    {#if dData.GCC_CODE21 === "1GSYD"}
    <div class="chart" >
      <h2>Greater Sydney</h2>
      <img src="/charts/sydney-profit-loss.svg" alt="Sydney Profit Loss"/>
      <h3>Percentage of profit-making resales: <span class="blue">96%</span></h3>
      <h3>Percentage of loss-making resales: <span class="red">4%</span></h3>
    </div>
    
    {/if}

  {#if dData.GCC_CODE21 === "2GMEL"}
    <h3>Greater Melbourne: Population of {suffixFormat(dData.population)}</h3>
  {/if}

  {#if dData.gcc_code === "3GBRI"}
    <h3>Greater Brisbane: Population of {suffixFormat(dData.population)}</h3>
  {/if}

  {#if dData.gcc_code === "4GADE"}
    <h3>Greater Adelaide: Population of {suffixFormat(dData.population)}</h3>
  {/if}



    </div>
    {/if}

  <style>

.chart {
  border: 2px;
  border-color: black;
}

    div {
  position: absolute;
  bottom: 30px;
  right: 0;
  text-align: left;
}

h2 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 0.25rem;
  color: black;
  padding-top: 2%;
  padding-bottom: 2%;
}

h3 {
  font-size: 1rem;
  font-weight: 200;
  color: black;
  padding-top: 2%;
  padding-bottom: 2%;
}

.blue {
color: #0f6cc9;
font-weight: 700;
}

.red {
  color: #c21616;
  font-weight: 700;
}

.chart img {
    display: block;
    max-width: 80%;   /* Make sure the SVG is not too large */
    max-height: 80%;  /* Constrain its height as well */
    margin: 0 auto;   /* Center the image inside the tooltip */
  }


  </style>