<script>
  let imageURL = '';
  let isFetching = false;

  async function fetchRandomCatImage() {
    if (isFetching) return;
    isFetching = true;

    try {
      const response = await fetch('https://api.thecatapi.com/v1/images/search', {
        headers: {
          'x-api-key': import.meta.env.VITE_CAT_API_KEY,
        },
      });

      const data = await response.json();
      imageURL = data[0].url;
    } catch (error) {
      console.error('Failed to fetch cat image:', error);
    } finally {
      isFetching = false;
    }
  }

  async function changeImage() {
    await fetchRandomCatImage();
  }

  // Fetch a random cat image when the component is mounted
  import { onMount } from 'svelte';
  onMount(fetchRandomCatImage);
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100vw;
    height: 100vh;
    background-color: skyblue;
  }

  .frame {
    width: 300px;
    height: 300px;
    border: 2px solid black;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  #catImage {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
  }

  .meow-button {
    margin-top: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>

<div class="container">
  <div class="frame">
    {#if imageURL}
      <img id="catImage" src={imageURL} alt="Cat" />
    {:else}
      <p>Loading cat image...</p>
    {/if}
  </div>
  <div class="meow-button">
    <button class="btn btn-primary" on:click={changeImage} disabled={isFetching}>
      {#if isFetching}
        Fetching...
      {:else}
        Meow
      {/if}
    </button>
  </div>
</div>

<!-- Add DaisyUI styles using a link tag in the HTML head -->
<head>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/daisyui@1.7.1/full.css" rel="stylesheet">
</head>
