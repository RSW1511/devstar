<script>
  import { createEventDispatcher } from 'svelte';

  export let image;
  export let isLoading;
  export let editedImage;

  const dispatch = createEventDispatcher();

  const applyGrayscale = async () => {
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

    // Apply grayscale filter
    ctx.filter = 'grayscale(100%)';
    ctx.drawImage(img, 0, 0, img.width, img.height);

    // Convert the canvas to a data URL
    editedImage = canvas.toDataURL();

    // Dispatch an event with the edited image data
    dispatch('grayscaleApplied', { editedImage });
  };
</script>

<button on:click={applyGrayscale}>Grayscale</button>
