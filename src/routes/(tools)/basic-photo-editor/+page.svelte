<svelte:head>
  <title>Photo Editor</title>
</svelte:head>
<script lang="ts">
  import Intro from '$lib/Intro.svelte';
  import { onMount } from 'svelte'
  import { Fileupload, Label, Helper } from 'flowbite-svelte';
  import '@pqina/pintura/pintura.css';
  import { getEditorDefaults } from '@pqina/pintura';
  import { PinturaEditor } from '@pqina/svelte-pintura';
  let uploadedImageSrc = null; // Store the uploaded image source
  let editedImageSrc = null;
  export let data;

      image.onload = () => {
        if(canvas){
        //const canvas = document.createElement('canvas');
        canvas.width = image.width;
        canvas.height = image.height;
        const ctx = canvas.getContext('2d');

        if (ctx) {
          ctx.drawImage(image, 0, 0);
          // Set the border size and color
          ctx.lineWidth = $borderSize;
          ctx.strokeStyle = $borderColor;

  function handleFileUpload(event) {
    const uploadedFile = event.target.files[0];
    if (uploadedFile) {
      uploadedImageSrc = URL.createObjectURL(uploadedFile);
    }
  }

          // Set the updated image with the border
          uploadedImage = canvas.toDataURL();
        }
      }
    };
    image.src= uploadedImage;
  }
}
async function handleFileInput( event ) {
    const file = event.target.files[0];
    if (file) {
      const imageUrl = URL.createObjectURL(file);
      const image = new Image();
      image.src = imageUrl;
      await image.decode(); // Ensure the image is loaded
      
      // Get a reference to the canvas element
      const canvas = document.createElement('canvas');
      const ctx = canvas.getContext('2d');
      if(ctx){
      // Set canvas dimensions to match the image
      canvas.width = image.width;
      canvas.height = image.height;
      ctx.drawImage(image, 0, 0);

      ctx.lineWidth = $borderSize;
      ctx.strokeStyle = $borderColor;
      ctx.strokeRect(0, 0, canvas.width, canvas.height);
      uploadedImage = canvas.toDataURL();

      URL.revokeObjectURL(imageUrl); // Clean up the object URL
      //applyBorder();
    }
  } 
}
// Function to handle changes in border size
function handleBorderSize(event) {
    $borderSize = event.target.value;
  }

  // Function to handle changes in border color
  function handleBorderColor(event) {
    $borderColor = event.target.value;
  }
  
  function downloadImage() {
    if (uploadedImage) {
      const link = document.createElement('a');
      link.href = uploadedImage;
      link.download = 'bordered_image.png';
      link.click();
    }
  }
  /*function toggleBackground() {
    backgroundColor.update((currentColor) =>
      currentColor === '#FFFFFF' ? '#000000' : '#FFFFFF'
    );
  }*/

  <Intro heading={data.meta.title} description={data.meta.description} />

<Intro heading={data.meta.title} description={data.meta.description} />

<main>
	<h1 class="text-white">Image Bordered Effect</h1>
  
	<input
	  type="file"
	  accept="image/*"
	  on:change={handleFileInput}
	  class="text-white"
    style="width: 500px"
	/>
  
	<label class="text-white">
	  Bordered Size:
	  <input
		type="range"
		min="1"
		max="50"
		step="1"
		bind:value={$borderSize}
	  />
	  {$borderSize} pixels
	</label>
  <div>
  <label class="text-white" >
	  Bordered Color:
	  <input
		type="color"
		bind:value={$borderColor}
	  />
	  {$borderColor}
	</label>
  </div>
  
  
	{#if uploadedImage}
	<div class="image-container">
		<h2 class="text-white"><b>BORDERED IMAGE</b></h2>
    
      <Label for="with_helper" class="pb-2">Upload file</Label>
      <Fileupload id="with_helper" class="mb-2" on:change={handleFileUpload} />
       <div class="flex justify-center">
     </div>
        <Helper>SVG, PNG, JPG or GIF (MAX. 800x400px).</Helper>
		
      
        <div style="height:80vh">
          <PinturaEditor
          {...getEditorDefaults()}
          src={uploadedImageSrc}
          />
          </div>
          <!-- Download Button -->
          <button on:click={saveEditedImage}>Download Edited Image</button>

          <!-- Provide a download link -->
          {#if editedImageSrc}
          <a href={editedImageSrc} download="edited_image.jpg">Download Edited Image</a>
          {/if}
