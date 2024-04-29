<template>
  <main>
    <section v-if="!state.croppedImage">
      <input type="file" accept="image/*" @change="onFileChange">      
      <cropper ref="cropperRef" :src="state.originalImage" :stencil-component='CircleStencil' />
      <button @click="onImageCrop">Crop</button>
    </section>

    <section v-if="state.croppedImage">
      Raw Image:
      <img :src="state.croppedImage" alt="Cropped Image" />
    </section>

    <section v-if="state.finalImage">
      Compressed Image:
      <img :src="state.finalImage" alt="Final Image">
    </section>
  </main>
</template>

<script setup lang="ts">
import { Cropper, CircleStencil } from 'vue-advanced-cropper';
import { reactive, ref } from 'vue';
import 'vue-advanced-cropper/dist/style.css';

const cropperRef = ref(null);

const state: {
  originalImage: string | null,
  croppedImage: string | null,
  finalImage: string | null
} = reactive({
  originalImage: null,
  croppedImage: null,
  finalImage: null,
})

const onFileChange = (event: Event) => {
  const { target } = event as Event & { target: EventTarget & HTMLInputElement };

  if (target?.files && target.files[0]) {
    state.originalImage = URL.createObjectURL(target.files[0]);
  }
}

const onImageCrop = () => {
  const { canvas } = cropperRef.value?.getResult();

  if (canvas) {
    state.croppedImage = canvas.toDataURL();
  }
}
</script>

<style scoped></style>
