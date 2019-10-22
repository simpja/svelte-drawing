<script>
  import { onMount } from "svelte";
  import ShowCoords from "./ShowCoords.svelte";
  import appricot from "./appricot.png";
  import banana from "./banana.png";

  let windowWidth = 0;
  let windowHeight = 0;
  let imgWidth;
  let imgHeight;
  let ctx;
  let canvasElement;
  let imgElement1;
  let imgElement2;
  let mouseDown = false;
  let mouseX = 0;
  let mouseY = 0;
  let imgSrc;

  function trackMouse({ x, y, type }) {
    if (type === "mousedown") {
      if (imgSrc === imgElement1) {
        imgSrc = imgElement2;
      } else {
        imgSrc = imgElement1;
      }
      mouseDown = true;
    }
    if (type === "mouseup") {
      mouseDown = false;
    }

    if (mouseDown) {
      drawBetween(mouseX, mouseY, x, y);
    }

    mouseX = x;
    mouseY = y;
  }

  function drawBetween(oldX, oldY, newX, newY) {
    const dx = oldX - newX;
    const dy = oldY - newY;
    const mag = Math.max(Math.abs(Math.sqrt(dx * dx + dy * dy)), 1);
    const nx = -1 * (dx / mag);
    const ny = -1 * (dy / mag);
    let count = 0;
    if (mag === count) {
      drawImage(newX, newY);
    } else {
      while (count < mag) {
        drawImage(oldX + nx * count, oldY + ny * count);
        count++;
      }
    }
  }

  function drawImage(x, y) {
    const dimensions = 200;
    const cx = x - dimensions / 2;
    const cy = y - dimensions / 2;

    // ctx.translate(cx, cy);
    // ctx.rotate((Math.PI / 180) * counter++);
    ctx.drawImage(imgSrc, cx, cy, dimensions, dimensions);
    // ctx.translate(-cx, -cy);
    // ctx.translate(-(x + dimensions / 2), -(y + dimensions / 2));
    // ctx.setTransform(1, 0, 0, 1, 0, 0);
    // ctx.resetTransform();
    // requestAnimationFrame(drawImage);
  }

  function resetDrawing() {
    ctx.clearRect(0, 0, windowWidth, windowHeight);
  }

  onMount(() => {
    ctx = canvasElement.getContext("2d");
  });
</script>

<style>
  canvas {
    cursor: crosshair;
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
    <ShowCoords x={mouseX} y={mouseY} />
    <button on:click={resetDrawing}>Reset</button>
  </div>

  <pre class="absolute right-0 mr3">{mouseDown}</pre>

  <div
    style="display: inline-block;"
    bind:clientWidth={imgWidth}
    bind:clientHeight={imgHeight}>
    <img
      class="dn"
      width="200"
      height="200"
      bind:this={imgElement1}
      src={appricot}
      alt="appricot1" />
    <img
      class="dn"
      width="200"
      height="200"
      bind:this={imgElement2}
      src={banana}
      alt="banana" />
  </div>

  <canvas
    on:mousemove={trackMouse}
    on:mouseup={trackMouse}
    on:mousedown={trackMouse}
    bind:this={canvasElement}
    width={windowWidth}
    height={windowHeight} />
</div>
