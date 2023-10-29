<script>
  import { createEventDispatcher } from 'svelte';

  export let image;
  export let isLoading;
  export let editedImage;

  const dispatch = createEventDispatcher();

  const applyRemoveBackground = async () => {
    // Placeholder logic for background removal (replace with your implementation)
    const img = new Image();
    img.src = image;

    await new Promise((resolve) => {
      img.onload = resolve;
    });

    const canvas = document.createElement('canvas');
    const ctx = canvas.getContext('2d');

    canvas.width = img.width;
    canvas.height = img.height;

    ctx.drawImage(img, 0, 0, img.width, img.height);

    // Example: Apply a simple background color
    ctx.fillStyle = '#FFFFFF'; // White background
    ctx.fillRect(0, 0, img.width, img.height);

    // Convert the canvas to a data URL
    editedImage = canvas.toDataURL();

    // Dispatch an event with the edited image data
    dispatch('removeBackgroundApplied', { editedImage });
  };
</script>

<button on:click={applyRemoveBackground}>Remove Background</button>
