<script lang="ts">
  import 'two-up-element'
  import { originalImage, modifiedImage } from './store'

  let processingImage = true
  let tries = 0
  let intervalId = 0

  $: {
    if (processingImage) {
      clearInterval(intervalId)
      intervalId = setInterval(() => {
        tries++
        const image = new Image()
        image.src = $modifiedImage
        image.onload = () => {
          processingImage = false
          clearInterval(intervalId)
        }
      }, 500)
    }
  }
</script>

<two-up>
  <!-- svelte-ignore a11y-img-redundant-alt -->
  <img src={$originalImage} alt="Original image" />
  <!-- svelte-ignore a11y-img-redundant-alt -->
  {#if processingImage}
    <div class="flex flex-col items-center justify-center">
      <div class="animate-spin rounded-full h-32 w-32 border-b-2 border-gray-900"></div>
      <p class="mt-4 text-2xl font-bold text-gray-900">
        Processing image...
      </p>
      <p class="mt-2 text-gray-900">
        This may take a while
      </p>
    </div>
  {:else}
    <img 
      src={$modifiedImage} 
      alt="Modified image" 
    />
  {/if}
</two-up>

<a 
  download="image-with-transparent-background.png" 
  href={$modifiedImage} 
  class="block bg-blue-500 w-full font-bold text-xl text-center px-4 py-2 hover:bg-blue-700 text-white rounded-full mt-10"
>
  Download image with transparent background
</a>