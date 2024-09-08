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
      <img src="/charts/sydney-profit-loss.svg" alt="Sydney Profit Loss"/>
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

.tooltip {
  position: absolute;
    background: rgba(var(--background-rgb), 0.7);
    backdrop-filter: blur(8px);
    padding: 10px;
    border-radius: 8px;
    box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.2);
    pointer-events: none;
    z-index: 100;
    color: black;
    border: 2px solid white;
    transform: translate(-50%, -50%);
    width: 40%;
    left: 10%;
    box-sizing: border-box;
  }

    div {
  position: absolute;
  bottom: 30px;
  right: 0;
  text-align: right;
}

h2 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 0.25rem;
  color: black;
}

h3 {
  font-size: 1.15rem;
  font-weight: 200;
  color: black;
}

.chart img {
    display: block;
    max-width: 80%;   /* Make sure the SVG is not too large */
    max-height: 80%;  /* Constrain its height as well */
    margin: 0 auto;   /* Center the image inside the tooltip */
  }


  </style>