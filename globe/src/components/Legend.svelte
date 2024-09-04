<script>
    export let colorScale;
    export let data;

    const minColor = colorScale.range()[0];
    const maxColor = colorScale.range()[1];

    const minValue= colorScale.domain()[0];
    const maxValue= colorScale.domain()[1];

    import { format } from "d3-format";
  const suffixFormat = (d) => format(".2~s")(d).replace(/G/, "B")
  $: percentOfMax = (data?.population / maxValue) * 100;
</script>

<div class="legend">
    <span class="label">{minValue}</span>
    <div
  class="bar"
  style:background="linear-gradient(to right, {colorScale.range()[0]}, {colorScale.range()[1]})"
/>
  {#if percentOfMax}
  <span class="line" style="left: {percentOfMax}%;" />
  {/if}
</div>

<style>
.bar {
  height: 15px;
  width: 100%;
  position: relative;
}

.legend {
  display: flex;
  flex-direction: row;
  gap: 6px;
  position: relative;
}

.label {
  color: white;
  font-size: 0.85rem;
  user-select: none;
}

.line {
  position: absolute;
  top: 0;
  height: 15px;
  width: 2px;
  background: white;
  transition: left 800ms cubic-bezier(1, 0, 0, 1);
}

</style>