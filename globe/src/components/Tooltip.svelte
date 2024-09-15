<script>
    export let dData;
    export let top;
  export let left;

    import { format } from "d3-format";
  const suffixFormat = (d) => format(".2~s")(d).replace(/G/, "B")

  import {fade} from "svelte/transition";

 
   $: adjustedTop = top;
  $: adjustedLeft = left;

  $: {
    if (typeof window !== "undefined") {
      const tooltipWidth = 300; 
      const tooltipHeight = 150; 

      const padding = 10; 

    
      if (left + tooltipWidth > window.innerWidth) {
        adjustedLeft = window.innerWidth - tooltipWidth - padding;
      } else if (left < 0) {
        adjustedLeft = padding;
      }

     
      if (top + tooltipHeight > window.innerHeight) {
        adjustedTop = window.innerHeight - tooltipHeight - padding;
      } else if (top < 0) {
        adjustedTop = padding;
      }
    }
  }
  </script>
  
  {#if dData}
    <div in:fade class="tooltip" style="top: {adjustedTop}px; left: {adjustedLeft}px;">
      

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
  <div class="chart" >
    <div class="header">
      <h2>Greater Melbourne</h2>
      <img src="/charts/melbourne-profit-loss.svg" alt="Melbourne Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">97.9%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 97.9%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">2.1%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 2.1%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "3GBRI"}
  <div class="chart" >
    <div class="header">
      <h2>Greater Brisbane</h2>
      <img src="/charts/brisbane-profit-loss.svg" alt="Brisbane Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">99.5%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 99.5%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">0.5%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 0.5%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "4GADE"}
  <div class="chart" >
    <div class="header">
      <h2>Greater Adelaide</h2>
      <img src="/charts/adelaide-profit-loss.svg" alt="Adelaide Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">96.8%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 96.8%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">3.2%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 3.2%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "1RNSW"}
  <div class="chart" >
    <div class="header">
      <h2>Regional NSW</h2>
      <img src="/charts/regional-NSW-profit-loss.svg" alt="Regional NSW Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">95.9%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 95.9%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">4.1%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 4.1%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "2RVIC"}
  <div class="chart" >
    <div class="header">
      <h2>Regional Victoria</h2>
      <img src="/charts/regional-victoria-profit-loss.svg" alt="Regional Victoria Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">98.4%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 98.4%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">1.6%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 1.6%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "3RQLD"}
  <div class="chart" >
    <div class="header">
      <h2>Regional Queensland</h2>
      <img src="/charts/regional-queensland-profit-loss.svg" alt="Regional Queensland Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">97.1%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 97.1%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">2.9%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 2.9%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "4RSAU"}
  <div class="chart" >
    <div class="header">
      <h2>Regional SA</h2>
      <img src="/charts/regional-SA-profit-loss.svg" alt="Regional SA Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">87.1%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 87.1%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">12.9%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 12.9%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "5GPER"}
  <div class="chart" >
    <div class="header">
      <h2>Greater Perth</h2>
      <img src="/charts/perth-profit-loss.svg" alt="Perth Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">97.1%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 95.9%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">2.9%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 2.9%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "5RWAU"}
  <div class="chart" >
    <div class="header">
      <h2>Regional WA</h2>
      <img src="/charts/regional-WA-profit-loss.svg" alt="Regional WA Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">92.6%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 92.6%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">7.4%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 7.4%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "6GHOB"}
  <div class="chart" >
    <div class="header">
      <h2>Greater Hobart</h2>
      <img src="/charts/hobart-profit-loss.svg" alt="Hobart Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">96.8%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 96.8%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">3.2%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 3.2%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "6RTAS"}
  <div class="chart" >
    <div class="header">
      <h2>Regional Tasmania</h2>
      <img src="/charts/regional-tasmania-profit-loss.svg" alt="Regional Tasmania Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">97.3%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 97.3%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">2.7%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 2.7%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "7GDAR"}
  <div class="chart" >
    <div class="header">
      <h2>Greater Darwin</h2>
      <img src="/charts/darwin-profit-loss.svg" alt="Darwin Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">88.4%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 88.4%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">11.6%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 11.6%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "7RNTE"}
  <div class="chart" >
    <div class="header">
      <h2>Regional NT</h2>
      <img src="/charts/regional-NT-profit-loss.svg" alt="Regional NT Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">92.8%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 92.8%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">7.2%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 7.2%;"></div>
      </div>
  </div>
  {/if}

  {#if dData.GCC_CODE21 === "8ACTE"}
  <div class="chart" >
    <div class="header">
      <h2>Greater Canberra</h2>
      <img src="/charts/canberra-profit-loss.svg" alt="Canberra Profit Loss" />
    </div>
    <div class="progress-bars">
      <h3>Percentage of profit-making resales: <span class="blue-text">94%</span></h3>
      <div class="bar-container">
        <div class="bar blue-bar" style="width: 94%;"></div>
      </div>
      </div>

      <h3>Percentage of loss-making resales: <span class="red-text">6%</span></h3>
      <div class="bar-container">
        <div class="bar red-bar" style="width: 6%;"></div>
      </div>
  </div>
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
  max-width: 50px; 
  height: auto;
  margin-left: auto;
}

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
  background-color: #0f6cc9; 
}

.red-bar {
  background-color: #c21616; 
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