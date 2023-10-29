<!-- main.svelte -->

<script>
	import { onMount } from 'svelte';
	import { Spinner } from 'flowbite-svelte';
	import Grayscale from './Grayscale.svelte';
	import RemoveBackground from './RemoveBackground.svelte';
	import Crop from './Crop.svelte';
	import AddBorder from './AddBorder.svelte';
	import Resize from './Resize.svelte';
  
	let image;
	let editedImage;
	let isLoading = false;
	let state = {
	  filters: [],
	  brightness: 50,
	  contrast: 50,
	  blur: 0,
	};
  
	// Define the callback function
	const handleB = () => {
	  // Handle the border application logic if needed
	  console.log('Border applied!');
	};
  
	const loadImage = (event) => {
	  const file = event.target.files[0];
  
	  if (file) {
		const reader = new FileReader();
  
		reader.onload = (e) => {
		  image = e.target.result;
		  state = {
			filters: [],
			brightness: 50,
			contrast: 50,
			blur: 0,
		  };
		};
  
		reader.readAsDataURL(file);
	  }
	};
  
	const applyMachineLearning = async () => {
	  isLoading = true;
  
	  // Simulate an asynchronous machine learning task
	  await new Promise((resolve) => setTimeout(resolve, 2000));
  
	  const canvas = document.createElement('canvas');
	  const ctx = canvas.getContext('2d');
	  const img = new Image();
	  img.src = image;
  
	  img.onload = () => {
		canvas.width = img.width;
		canvas.height = img.height;
  
		// Apply previous filters
		for (const filter of state.filters) {
		  ctx.filter = filter;
		  ctx.drawImage(img, 0, 0, img.width, img.height);
		}
  
		// Apply current sliders
		ctx.filter = `brightness(${state.brightness}%) contrast(${state.contrast}%) blur(${state.blur}px)`;
		ctx.drawImage(img, 0, 0, img.width, img.height);
  
		editedImage = canvas.toDataURL();
		isLoading = false;
	  };
	};
  
	onMount(() => {
	  // Provide a default image or load from the server on mount
	  // Replace the placeholder URL with the actual URL or leave it as is
	  image = 'https://example.com/default-image.jpg';
	});
  </script>
  
  <main>
	<div class="container">
	  <input type="file" accept="image/*" on:change={loadImage} />
	  {#if image}
		<div class="image-section">
		  <img src={image} alt="Original" />
		</div>
  
		{#if isLoading}
		  <div class="spinner-section">
			<Spinner color="black" />
		  </div>
		{:else if editedImage}
		  <div class="image-section">
			<img src={editedImage} alt="Edited" />
		  </div>
		{/if}
  
		{#if !isLoading}
		  <div class="button-section">
			<div class="technique-buttons">
			  <Grayscale image={image} isLoading={isLoading} editedImage={editedImage} />
			  <RemoveBackground image={image} isLoading={isLoading} editedImage={editedImage} />
			  <Crop image={image} isLoading={isLoading} editedImage={editedImage} />
			  <AddBorder image={image} isLoading={isLoading} editedImage={editedImage} on:borderApplied={handleB} />
			  <Resize image={image} isLoading={isLoading} editedImage={editedImage} />
			  <!-- Add other technique buttons here -->
			</div>
  
			<div class="sliders">
			  <label>Brightness:</label>
			  <input type="range" min="0" max="100" bind:value={state.brightness} />
  
			  <label>Contrast:</label>
			  <input type="range" min="0" max="100" bind:value={state.contrast} />
  
			  <label>Blur:</label>
			  <input type="range" min="0" max="20" bind:value={state.blur} />
  
			  <button on:click={applyMachineLearning}>Apply Filters</button>
			</div>
		  </div>
		{/if}
	  {/if}
	</div>
  </main>
  
  <style>
	.container {
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	}
  
	.image-section,
	.spinner-section,
	.button-section {
	  margin-top: 20px;
	}
  
	img {
	  max-width: 100%;
	}
  
	button {
	  margin-top: 10px;
	  margin-right: 5px;
	  padding: 10px;
	  background-color: #4caf50;
	  color: white;
	  border: none;
	  border-radius: 5px;
	  cursor: pointer;
	}
  
	.technique-buttons {
	  display: flex;
	  flex-wrap: wrap;
	  gap: 10px;
	}
  
	.sliders {
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	}
  </style>
  