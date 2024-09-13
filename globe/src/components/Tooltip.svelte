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
      <div class="header">
        <h2>Greater Sydney</h2>
        <img src="/charts/sydney-profit-loss.svg" alt="Sydney Profit Loss" />
      </div>
      <div class="progress-bars">
        <h3>Percentage of profit-making resales: <span class="blue-text">96%</span></h3>
        <div class="bar-container">
          <div class="bar blue-bar" style="width: 96%;"></div>
        </div>
        </div>

        <h3>Percentage of loss-making resales: <span class="red-text">4%</span></h3>
        <div class="bar-container">
          <div class="bar red-bar" style="width: 4%;"></div>
        </div>
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
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 10px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  z-index: 1000;
  width: 300px;
}

/* Flexbox for inline layout of h2 and pie chart */
.header {
  display: flex;
  align-items: center;
}

h2 {
  font-size: 1.2rem;
  font-weight: 700;
  margin-right: 10px;
  color: black;
}

.chart img {
  max-width: 50px; /* Make the pie chart smaller */
  height: auto;
  margin-left: auto;
}

/* Progress bars for percentage display */
.progress-bars {
  margin-top: 10px;
}

.bar-container {
  width: 100%;
  background-color: #eee;
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 10px;
}

.bar {
  height: 15px;
  border-radius: 4px;
}

.blue-bar {
  background-color: #0f6cc9; /* Blue for profit */
}

.red-bar {
  background-color: #c21616; /* Red for loss */
}

h3 {
  font-size: 1rem;
  font-weight: 400;
  margin-bottom: 5px;
  color: black;
}

.blue-text {
color: #c21616;
font-weight: 700;

}

.red-text {
color: #0f6cc9;
font-weight: 700;
}

  </style>