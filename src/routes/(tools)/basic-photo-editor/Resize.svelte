<script>
  import { createEventDispatcher } from 'svelte';

  export let image;
  export let isLoading;
  export let editedImage;

  const dispatch = createEventDispatcher();
  let newWidth = 200; // Default width
  let newHeight = 200; // Default height

  const applyResize = async () => {
    const img = new Image();
    img.src = image;

    await new Promise((resolve) => {
      img.onload = resolve;
    });

    const canvas = document.createElement('canvas');
    const ctx = canvas.getContext('2d');

    canvas.width = newWidth;
    canvas.height = newHeight;

    // Draw the resized image
    ctx.drawImage(img, 0, 0, newWidth, newHeight);

    // Convert the canvas to a data URL
    editedImage = canvas.toDataURL();

    // Dispatch an event with the resized image data
    dispatch('resizeApplied', { editedImage });
  };
</script>

<div>
  <label for="newWidth">New Width:</label>
  <input type="number" bind:value={newWidth} min="1" />

  <label for="newHeight">New Height:</label>
  <input type="number" bind:value={newHeight} min="1" />

  <button on:click={applyResize}>Resize</button>
</div>
