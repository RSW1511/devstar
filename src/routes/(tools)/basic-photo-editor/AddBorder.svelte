<!-- AddBorder.svelte -->

<script>
    import { createEventDispatcher } from 'svelte';
  
    export let image;
    export let isLoading;
    export let editedImage;
  
    const dispatch = createEventDispatcher();
    let selectedBorderStyle = 'solid';
    let borderSize = 10;
    let borderColor = '#FFFFFF'; // Default color is white
  
    const applyAddBorder = async () => {
      // Implement the logic for adding border
      const canvas = document.createElement('canvas');
      const ctx = canvas.getContext('2d');
      const img = new Image();
      img.src = image;
  
      img.onload = () => {
        canvas.width = img.width + 2 * borderSize;
        canvas.height = img.height + 2 * borderSize;
  
        // Draw the border
        ctx.fillStyle = borderColor;
        ctx.fillRect(0, 0, canvas.width, canvas.height);
  
        // Draw the image on top of the border
        ctx.drawImage(img, borderSize, borderSize);
  
        editedImage = canvas.toDataURL();
        dispatch('borderApplied', { editedImage });
      };
    };
  </script>
  
  <div>
    <label for="borderStyle">Border Style:</label>
    <select bind:value={selectedBorderStyle}>
      <option value="solid">Solid</option>
      <option value="dashed">Dashed</option>
      <option value="dotted">Dotted</option>
    </select>
  
    <label for="borderSize">Border Size:</label>
    <input type="number" bind:value={borderSize} min="1" />
  
    <label for="borderColor">Border Color:</label>
    <input type="color" bind:value={borderColor} />
  
    <button on:click={applyAddBorder}>Apply Border</button>
  </div>
  