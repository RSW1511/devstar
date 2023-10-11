<script lang="ts">
	import Intro from '$lib/Intro.svelte';
	export let data;

	let uploadedImage;
	let newWidth = 0;
	let maxWidth = 800; // Set your desired maximum width for resizing

	async function handleFileInput(event) {
		const file = event.target.files[0];
		if (file) {
			const imageUrl = URL.createObjectURL(file);
			const image = new Image();
			image.src = imageUrl;
			await image.decode(); // Ensure the image is loaded

			// Calculate the new dimensions while preserving the aspect ratio
			let newWidth = image.width;
			let newHeight = image.height;

			if (newWidth > maxWidth) {
				const ratio = maxWidth / newWidth;
				newWidth = maxWidth;
				newHeight = newHeight * ratio;
			}

			// Get a reference to the canvas element
			const canvas = document.createElement('canvas');
			const ctx = canvas.getContext('2d');

			// Set canvas dimensions to match the resized image
			canvas.width = newWidth;
			canvas.height = newHeight;

			// Draw the resized image
			ctx.drawImage(image, 0, 0, newWidth, newHeight);

			// Replace the uploadedImage with the resized image data URL
			uploadedImage = canvas.toDataURL();

			URL.revokeObjectURL(imageUrl); // Clean up the object URL
		}
	}

	function downloadImage() {
		if (uploadedImage) {
			const link = document.createElement('a');
			link.href = uploadedImage;
			link.download = 'resized_image.png';
			link.click();
		}
	}
</script>

<svelte:head>
	<title>Photo Editor</title>
</svelte:head>

<Intro heading={data.meta.title} description={data.meta.description} />

<main>
	<h1 class="text-white">Image Resizer</h1>

	<input type="file" accept="image/*" on:change={handleFileInput} class="text-white" />

	<label class="text-black">
		New Width:
		<input type="number" min="1" bind:value={newWidth} />
		pixels
	</label>

	{#if uploadedImage}
		<div class="image-container">
			<h2 class="text-white">Resized Image</h2>
			<img src={uploadedImage} alt="" />
			<button
				on:click={downloadImage}
				class="mt-4 text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
				>Download Resized Image</button
			>
		</div>
	{/if}
</main>

<style>
	main {
		text-align: center;
		margin: 20px;
	}
	img {
		max-width: 100%;
		margin-top: 10px;
	}
	.image-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
	}
</style>
