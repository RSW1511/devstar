<!-- LazyImage.svelte -->

<script>
  import { onMount } from 'svelte';

  export let src;
  export let alt;
  
  let image;

  onMount(() => {
    const img = document.querySelector('img');
    img.src = src;
  });
</script>

<svelte:head>
  <style>
    img {
      display: block;
      max-width: 100%;
      height: auto;
    }
  </style>
</svelte:head>

<div>
  <img src="" alt={alt} loading="lazy" />
</div>
