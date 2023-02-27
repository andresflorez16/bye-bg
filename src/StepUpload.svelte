<script lang="ts">
  import { ImageStatus } from '../types.d'
  import { imageStatus, modifiedImage, originalImage } from './store'
  import { Cloudinary } from '@cloudinary/url-gen'
  import { backgroundRemoval } from '@cloudinary/url-gen/actions/effect'
  import Dropzone from 'dropzone'
  import 'dropzone/dist/dropzone.css'
  
  import { onMount } from 'svelte'
  
  const cloudinary = new Cloudinary({
    cloud: {
      cloudName: 'drusmckql'
    },
    url: {
      secure: true
    }
  })

  onMount(() => {
    const dropzone = new Dropzone('#dropzone', {
      uploadMultiple: false,
      acceptedFiles: '.jpg, .png, .jpeg, .webp',
      maxFiles: 1
    })

    dropzone.on('sending', (file, xhr, formData) => {
      imageStatus.set(ImageStatus.UPLOADING)
      formData.append('upload_preset', 'bye-bg')
      formData.append('timestamp', (Date.now() / 1000))
      formData.append('api_key', '176243884338915')
    })

    dropzone.on('success', (file, response) => {
      const { secure_url: url, public_id: id } = response

      const imageWithoutBackground = cloudinary
        .image(id)
        .effect(backgroundRemoval())


      modifiedImage.set(imageWithoutBackground.toURL())
      originalImage.set(url)
      imageStatus.set(ImageStatus.DONE)
    })
  })
</script>

<form 
  id="dropzone"
  class="shadow-2xl border-dashed border-2 border-gray-300 rounded-lg aspect-video w-full flex items-center justify-center flex-col p-10" 
  action="https://api.cloudinary.com/v1_1/drusmckql/image/upload"
>
  {#if $imageStatus === ImageStatus.READY}
  <button class="font-bold pointer-events-none rounded-full bg-green-700 text-white text-xl px-6 py-4">
    Upload files here...
  </button>
  <strong class="text-lg mt-4">or drop a file</strong>
  {/if}
  {#if $imageStatus === ImageStatus.UPLOADING}
  <div class="flex flex-col items-center">
    <div class="animate-spin rounded-full h-32 w-32 border-b-2 border-gray-900"></div>
    <strong class="text-lg">Uploading...</strong>
  </div>
  {/if}
</form>