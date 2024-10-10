<script lang="ts">
  import { onMount } from 'svelte';
  import convert from 'color-convert';

  // HSV values
  let hue = 0;
  let saturation = 100;
  let value = 100;

  // Color formats
  let hexColor = '';
  let rgbColor = '';
  let cmykColor = '';
  let hslColor = '';
  let canvas: HTMLCanvasElement;
  let dragging = false;

  // Reactive statement to create the HSV string
  $: hsvString = `${hue}, ${saturation}, ${value}`;

  onMount(() => {
    // Initialize canvas or set up other properties here
    const context = canvas.getContext('2d');
    if (context) {
      drawCanvas();
    }
  });

  function drawCanvas() {
    const { r, g, b } = hsvToRgb(hue, saturation, value);
    const context = canvas.getContext('2d');
    if (context) {
      context.fillStyle = `rgb(${r}, ${g}, ${b})`;
      context.fillRect(0, 0, canvas.width, canvas.height);
    }
    updateColorFormats();
  }

  function hsvToRgb(h: number, s: number, v: number) {
    let r, g, b;
    const i = Math.floor(h / 60);
    const f = h / 60 - i;
    const p = v * (1 - s / 100);
    const q = v * (1 - f * (s / 100));
    const t = v * (1 - (1 - f) * (s / 100));

    switch (i % 6) {
      case 0: (r = v), (g = t), (b = p); break;
      case 1: (r = q), (g = v), (b = p); break;
      case 2: (r = p), (g = v), (b = t); break;
      case 3: (r = p), (g = q), (b = v); break;
      case 4: (r = t), (g = p), (b = v); break;
      case 5: (r = v), (g = p), (b = q); break;
    }

    return { r: Math.round(r * 255), g: Math.round(g * 255), b: Math.round(b * 255) };
  }

  function onCanvasClick(event: MouseEvent) {
    const rect = canvas.getBoundingClientRect();
    const x = event.clientX - rect.left;
    const y = event.clientY - rect.top;
    
    // Calculate saturation and value based on mouse position
    saturation = Math.round((x / canvas.width) * 100);
    value = Math.round((1 - y / canvas.height) * 100);
    
    drawCanvas();
  }

  function onCanvasMouseDown(event: MouseEvent) {
    dragging = true;
    onCanvasClick(event);
  }

  function onCanvasMouseUp() {
    dragging = false;
  }

  function updateColorFormats() {
    const rgb = convert.hsv.rgb(hue, saturation, value);
    const hex = convert.hsv.hex(hue, saturation, value);
    const cmyk = convert.hsv.cmyk(hue, saturation, value);
    const hsl = convert.hsv.hsl(hue, saturation, value);

    rgbColor = `rgb(${rgb.join(', ')})`;
    hexColor = `#${hex}`;
    cmykColor = `cmyk(${cmyk.join(', ')})`;
    hslColor = `hsl(${hsl.join(', ')})`;
  }

  function handleHexInput(event: Event) {
    const hexValue = (event.target as HTMLInputElement).value.replace('#', '');
    const rgb = convert.hex.rgb(hexValue);
    const hsv = convert.rgb.hsv(...rgb);

    hue = hsv[0];
    saturation = hsv[1];
    value = hsv[2];

    drawCanvas();
  }

  function handleRgbInput(event: Event) {
    const rgbValue = (event.target as HTMLInputElement).value.match(/\d+/g);
    if (rgbValue && rgbValue.length === 3) {
      const hsv = convert.rgb.hsv(parseInt(rgbValue[0]), parseInt(rgbValue[1]), parseInt(rgbValue[2]));
      
      hue = hsv[0];
      saturation = hsv[1];
      value = hsv[2];

      drawCanvas();
    }
  }

  function handleCmykInput(event: Event) {
    const cmykValue = (event.target as HTMLInputElement).value.match(/\d+/g);
    if (cmykValue && cmykValue.length === 4) {
      const rgb = convert.cmyk.rgb(
        parseInt(cmykValue[0]),
        parseInt(cmykValue[1]),
        parseInt(cmykValue[2]),
        parseInt(cmykValue[3])
      );
      const hsv = convert.rgb.hsv(...rgb);

      hue = hsv[0];
      saturation = hsv[1];
      value = hsv[2];

      drawCanvas();
    }
  }

  function handleHslInput(event: Event) {
    const hslValue = (event.target as HTMLInputElement).value.match(/\d+/g);
    if (hslValue && hslValue.length === 3) {
      const rgb = convert.hsl.rgb(
        parseInt(hslValue[0]),
        parseInt(hslValue[1]),
        parseInt(hslValue[2])
      );
      const hsv = convert.rgb.hsv(...rgb);

      hue = hsv[0];
      saturation = hsv[1];
      value = hsv[2];

      drawCanvas();
    }
  }

  function handleHsvInput(event: Event) {
    const hsvValue = (event.target as HTMLInputElement).value.match(/\d+/g);
    if (hsvValue && hsvValue.length === 3) {
      hue = parseInt(hsvValue[0]);
      saturation = parseInt(hsvValue[1]);
      value = parseInt(hsvValue[2]);

      drawCanvas();
    }
  }
</script>

<div class="color-picker">
  <div class="canvas-container">
    <canvas bind:this={canvas} width="300" height="150" on:mousedown={onCanvasMouseDown} on:mouseup={onCanvasMouseUp} on:mouseleave={onCanvasMouseUp} on:click={onCanvasClick}></canvas>
  </div>

  <div class="sliders">
    <label for="hue">Hue</label>
    <input id="hue" type="range" min="0" max="360" bind:value={hue} on:input={drawCanvas} />

    <label for="saturation">Saturation</label>
    <input id="saturation" type="range" min="0" max="100" bind:value={saturation} on:input={drawCanvas} />

    <label for="value">Value</label>
    <input id="value" type="range" min="0" max="100" bind:value={value} on:input={drawCanvas} />
  </div>

  <div class="color-formats">
    <label for="hexColor">HEX</label>
    <input id="hexColor" type="text" bind:value={hexColor} on:input={handleHexInput} />

    <label for="rgbColor">RGB</label>
    <input id="rgbColor" type="text" bind:value={rgbColor} on:input={handleRgbInput} />

    <label for="cmykColor">CMYK</label>
    <input id="cmykColor" type="text" bind:value={cmykColor} on:input={handleCmykInput} />

    <label for="hslColor">HSL</label>
    <input id="hslColor" type="text" bind:value={hslColor} on:input={handleHslInput} />

    <label for="hsvColor">HSV</label>
    <input id="hsvColor" type="text" bind:value={hsvString} on:input={handleHsvInput} />
  </div>
</div>

<style>
  .color-picker {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
  }

  .canvas-container {
    margin-bottom: 1rem;
  }

  .sliders {
    display: flex;
    flex-direction: column;
  }

  label {
    margin-bottom: 0.5rem;
  }

  input[type="range"] {
    margin-bottom: 1rem;
    width: 100%;
  }

  .color-formats {
    display: flex;
    flex-direction: column;
    margin-top: 1rem;
  }

  input[type="text"] {
    margin-bottom: 1rem;
    width: 100%;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
</style>
