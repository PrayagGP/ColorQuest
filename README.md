# ColorQuest

## About the Task

This task involves building an HSV-based color picker application using **Svelte**. The color picker is inspired by Google Color Picker and allows manipulation of three degrees of freedom: **hue**, **saturation**, and **value**. It also supports five color formats: **HEX**, **RGB**, **CMYK**, **HSL**, and **HSV**. Users can interact with sliders and input fields to change the color, which updates across all formats in real time. The application includes a canvas for color visualization and mouse interactions to adjust saturation and value.

## Installation

To run the project locally, follow these steps:

1. **Install Node.js and npm**  
   Ensure that Node.js and npm are installed on your system. You can download them from [here](https://nodejs.org/).

2. **Create a New Svelte Project**
   If you don’t already have a Svelte project, you can set one up with:
   ```bash
   npx degit sveltejs/template color-picker
   cd color-picker
   npm install

3. Add Color Conversion Library
This application uses the color-convert package to handle conversions between color formats. Install it by running:
npm install color-convert

4. Place the Provided Code
Copy the provided code into the +page.svelte file located in the src/routes/ directory of your Svelte project.

5. Run the Application
After placing the code, run the application with:
npm run dev

This will start the development server, and you can view the application by navigating to http://localhost:5173 in your browser.

### Prerequisites:
1. Node.js: Ensure Node.js is installed on your system.
2. Svelte: Familiarity with Svelte and how Svelte components work.
3. HTML Canvas: Understanding of how the HTML canvas element works, as it's used to render the color space.

### Explanation of code:
- <script lang="ts">
Svelte Imports: We import the onMount lifecycle function from Svelte to handle canvas initialization and the color-convert library for converting between color formats.

- State Variables: Variables like hue, saturation, and value hold the current HSV values. Other variables like hexColor, rgbColor, cmykColor, and hslColor store the respective color format values. The canvas variable binds to the HTML <canvas> element, and dragging tracks whether the user is clicking and dragging on the canvas.

- Reactive Statement: The $: syntax in Svelte is used to automatically re-calculate the hsvString when hue, saturation, or value changes.

- onMount Lifecycle Hook: The onMount function is called when the component is mounted. It initializes the canvas and draws the current color.

- drawCanvas Function: Draws the color on the canvas using the hsvToRgb function to convert the HSV values to RGB. It also calls updateColorFormats to update all color format inputs.

Input Handlers:

- handleHexInput, handleRgbInput, handleCmykInput, handleHslInput, and handleHsvInput allow the user to input color values in their respective formats. These functions parse the input and convert it to HSV for consistent color manipulation.

Canvas Event Handlers:
- onCanvasClick: Calculates the saturation and value based on the mouse position when the user clicks on the canvas. It updates the displayed color accordingly.
- onCanvasMouseDown and onCanvasMouseUp: Handle mouse drag events for more interactive control over saturation and value.

HTML Structure:
- Canvas: The canvas displays the selected color and allows users to interact with it by clicking or dragging.
- Sliders: Three sliders allow the user to adjust the hue, saturation, and value. Changes to these sliders update the canvas in real time.
- Color Format Inputs: The application supports five color formats (HEX, RGB, CMYK, HSL, and HSV). Users can input any valid value, and the color updates instantly across all formats.

"<style>":
- The CSS defines the layout and appearance of the application. Flexbox is used for centering elements and organizing the layout, while margins and paddings ensure proper spacing.   

### Running the Application

Once the code is placed in the +page.svelte file inside the src/routes folder, run the following command to start the Svelte development server:

npm run dev
This will launch the app at http://localhost:5173, where you can interact with the HSV color picker.

### Note
The server port number depends on your system and it will be visible on your terminal.
![image](https://github.com/user-attachments/assets/f2c001de-5279-4bca-bbb1-b980882acc8b)

### Working of the Color Picker
A [video](./ColorQuestDemo.mp4) demonstrating the working of the tool has been uploaded in this repository.


