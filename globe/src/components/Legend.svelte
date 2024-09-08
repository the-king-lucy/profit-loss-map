<script>
    const minColor = "#ffffff";
    const maxColor = "#0F6CC9";

    console.log("Hello from Legend")
 
</script>

<div class="legend">
  <span class="label">Less Profitable</span>
  <div
    class="bar"
    style="background: linear-gradient(to right, {minColor}, {maxColor});"
  ></div>
  <span class="label">More Profitable</span>
</div>

<style>
.bar {
  height: 15px;
  width: 100%;
  padding-left: 5%;
  padding-right: 5%;
  position: relative;
}

.legend {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100vw;
  margin: 20px;
  flex-direction: row;
  gap: 6px;
  position: relative;
}

.label {
  color: black;
  font-size: 0.85rem;
  user-select: none;
  white-space: nowrap; /* Prevents labels from wrapping */
  margin-left: 10px;
  margin-right: 10px;
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