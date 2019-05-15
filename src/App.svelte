<script>
  import { onMount } from "svelte";
  import ShowCoords from "./ShowCoords.svelte";
  import appricot from "./appricot.png";

  let windowWidth = 0;
  let windowHeight = 0;
  let imgWidth;
  let imgHeight;
  let canvasElement;
  let imgElement;
  let mouseDown = false;
  let coords = [];
  const mousePos = {
    x: 0,
    y: 0
  };

  function trackMouse({ x, y, type }) {
    if (type === "mousedown") {
      mouseDown = true;
    }
    if (type === "mouseup") {
      mouseDown = false;
    }
    if (mouseDown) {
      mousePos.x = x;
      mousePos.y = y;
      coords.push([x, y]);
    }
  }

  function drawImage() {
    const dimens = 100;

    console.log(coords);
    const ctx = canvasElement.getContext("2d");
    // coords.reduce((prev, next) => , [])

    coords.forEach(coord => {
      // TODO: Calculate a vector between prev and current coordinate. Draw for every pixel in that vector

      ctx.drawImage(
        imgElement,
        coord[0] - dimens / 2,
        coord[1] - dimens,
        dimens,
        dimens
      );
    });
  }

  function resetDrawing() {
    coords = [];
    const ctx = canvasElement.getContext("2d");
    ctx.clearRect(0, 0, windowWidth, windowHeight);
  }

  onMount(() => {
    const ctx = canvasElement.getContext("2d");
  });
</script>

<style>
	h1 {
	  color: purple;
	}
</style>

<!-- Bind size of window to local varaibles -->
<svelte:window bind:innerWidth={windowWidth} bind:innerHeight={windowHeight} />

<div class="flex-auto"> 

  <div class="absolute">
    <ShowCoords x={windowWidth} y={windowHeight}>
      <span slot="x">windowWidth</span>
      <span slot="y">windowHeight</span>
    </ShowCoords>
    <ShowCoords {...mousePos} />
    <button on:click="{drawImage}">Draw</button>
    <button on:click="{resetDrawing}">Reset</button>
  </div>

  <pre class="absolute right-0 mr3" >{mouseDown}</pre>

<div style="display: inline-block;"  bind:clientWidth={imgWidth} bind:clientHeight={imgHeight}>
  <img class="dn" width="200" height="200" bind:this={imgElement} src={appricot}  alt="appricot1" />
</div>

  <canvas on:mousemove={trackMouse} on:mouseup={trackMouse} on:mousedown={trackMouse} bind:this={canvasElement} width={windowWidth} height={windowHeight}></canvas>
</div>
