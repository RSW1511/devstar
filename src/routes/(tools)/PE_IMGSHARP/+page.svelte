<script>
    let sharpenAmount = 0;
    let imageUrl = "";
  
    function sharpenImage() {
      
      function sharpenImage() {
    
    const image = new Image();
   
    image.crossOrigin = "Anonymous";
   
    
    image.src = imageUrl;
   
    image.onload = () => {
      const canvas = document.createElement("canvas");
      const ctx = canvas.getContext("2d");
   
      canvas.width = image.width;
      canvas.height = image.height;
   
      ctx.drawImage(image, 0, 0);
   
      const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
      const data = imageData.data;
   
      const sharpenedData = sharpenImageData(data, canvas.width, canvas.height, sharpenAmount);
   
      ctx.putImageData(new ImageData(sharpenedData, canvas.width, canvas.height), 0, 0);
   
      const sharpenedImageUrl = canvas.toDataURL();
   
      imageUrl = sharpenedImageUrl;
    };
  }
   
  function sharpenImageData(data, width, height, amount) {
    const sharpenedData = new Uint8ClampedArray(data.length);
    const matrix = [
      [-1, -1, -1],
      [-1, 9 + amount / 10, -1],
      [-1, -1, -1],
    ];
   
    for (let y = 0; y < height; y++) {
      for (let x = 0; x < width; x++) {
        const i = (y * width + x) * 4;
        const r = applyConvolutionFilter(data, matrix, x, y, width, height, 0);
        const g = applyConvolutionFilter(data, matrix, x, y, width, height, 1);
        const b = applyConvolutionFilter(data, matrix, x, y, width, height, 2);
   
        sharpenedData[i] = r;
        sharpenedData[i + 1] = g;
        sharpenedData[i + 2] = b;
        sharpenedData[i + 3] = data[i + 3]; 
      }
    }
   
    return sharpenedData;
  }
   
  function applyConvolutionFilter(data, matrix, x, y, width, height, offset) {
    let result = 0;
   
    for (let j = -1; j <= 1; j++) {
      for (let i = -1; i <= 1; i++) {
        const pixelX = x + i;
        const pixelY = y + j;
   
        if (pixelX >= 0 && pixelX < width && pixelY >= 0 && pixelY < height) {
          const pixelIndex = (pixelY * width + pixelX) * 4;
          const weight = matrix[j + 1][i + 1];
          result += data[pixelIndex + offset] * weight;
        }
      }
    }
   
    return Math.max(0, Math.min(result, 255));
  }
  
    }
  </script>
  
  <style>
    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem;
    }
  
    .input-container {
      display: flex;
      align-items: center;
      margin-bottom: 1rem;
    }
  
    .input-container label {
      margin-right: 0.5rem;
    }
  
    .input-container input[type="text"] {
      padding: 0.5rem;
      border: 1px solid #ccc;
    }
  
    .image-container {
      margin-bottom: 1rem;
    }
  
    .image-container img {
      max-width: 100%;
      box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
    }
    
    .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
  }
  
  .input-container {
    display: flex;
    align-items: center;
    margin-bottom: 1rem;
  }
  
  .input-container label {
    margin-right: 0.5rem;
  }
  
  .input-container input[type="text"] {
    padding: 0.5rem;
    border: 1px solid #ccc;
  }
  
  .image-container {
    margin-bottom: 1rem;
  }
  
  .image-container img {
    max-width: 100%;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  }
  
  </style>
  
  <main class="container" >
    <h1>Image Sharpening Tool</h1>
  
    <div class="input-container">
      <label for="image-url">Image URL:</label>
      <input type="text" id="image-url" bind:value={imageUrl} />
    </div>
  
    <div class="input-container">
      <label for="sharpen-amount">Sharpen Amount:</label>
      <input
        type="range"
        id="sharpen-amount"
        min="0"  
        max= "100"
        step="1"
        bind:value={sharpenAmount}
      />
      <span>{sharpenAmount}</span>
    </div>
  
    <button on:click={sharpenImage}>Sharpen Image</button>
  
    <div class="image-container">
      {#if imageUrl}
        <img src={imageUrl} alt="Sharpened Image" style={`filter: blur(${sharpenAmount}px)`} />
      {/if}
    </div>
  </main>
  