<!-- InvertColors.svelte -->

<script>
  import { onMount } from 'svelte';

  export let image;
  export let isLoading;
  export let editedImage;

  const applyInvertColors = async () => {
    // Implement the logic for inverting colors
    // You can use a similar approach as in the main code
  };
</script>

<button on:click={applyInvertColors}>Invert Colors</button>
