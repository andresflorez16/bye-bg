<script lang='ts'>
  import { ImageStatus } from '../types.d'
  import StepEdit from './StepEdit.svelte'
  import StepUpload from './StepUpload.svelte'
  import { imageStatus } from './store'
</script>

<div class="grid place-content-center grid-cols-1 m-auto max-w-xl p-3 w-full h-screen">
  <header class="text-center py-10">
    <h1 class="text-6xl text-green-700 font-bold tracking-tighter">
      bye<span class="text-blue-900">bg</span>
    </h1>
  </header>

  <main>
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </main>



  <footer class="absolute bottom-0 max-w-xl w-full py-3">
    <p class="text-center text-gray-500 text-sm">
      Made with ❤️ by <a href="https://github.com/andresflorez16" class="text-blue-900">Andres Florez</a>
    </p>
  </footer>
</div>