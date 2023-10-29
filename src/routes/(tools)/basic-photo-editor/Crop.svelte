<!-- Crop.svelte -->

<script>
  import { createEventDispatcher, onMount, onDestroy } from 'svelte';
  import Cropper from 'cropperjs';
  import 'cropperjs/dist/cropper.min.css';

  export let image;
  export let isLoading;
  export let editedImage;
  export let nextImage; // New property to store the cropped image for further editing

  const dispatch = createEventDispatcher();
  let cropper;

  const applyCrop = async () => {
    // Get the cropped data
    const croppedData = cropper.getData();
    const canvas = cropper.getCroppedCanvas();

    // Convert the cropped canvas to a data URL
    editedImage = canvas.toDataURL();

    // Set the cropped image as the new base image for further editing
    nextImage = editedImage;

    // Create a downloadable link
    const downloadLink = document.createElement('a');
    downloadLink.href = editedImage;
    downloadLink.download = 'cropped_image.png'; // You can set the desired filename
    document.body.appendChild(downloadLink);
    downloadLink.click();
    document.body.removeChild(downloadLink);

    // Dispatch the event with the edited image
    dispatch('cropApplied', { editedImage });
  };

  onMount(() => {
    // Initialize cropper when the component is mounted
    const imageElement = document.getElementById('cropper-image');
    cropper = new Cropper(imageElement, {
      viewMode: 2, // Set the view mode as you need
      // Add other options based on your requirements
    });

    // Cleanup the cropper instance when the component is destroyed
    onDestroy(() => {
      cropper.destroy();
    });
  });
</script>

<div>
  {#if image}
    <img id="cropper-image" src={image} alt="Original" />
  {/if}

  <button on:click={applyCrop}>Apply Crop</button>

  {#if nextImage}
    <!-- Render the next editing components, passing nextImage as the image prop -->
    <SomeNextEditingComponent image={nextImage} />
  {/if}
</div>
