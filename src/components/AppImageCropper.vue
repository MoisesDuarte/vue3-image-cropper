<template>
  <main>
    <section v-if="!state.croppedImage">
      <input type="file" accept="image/*" @change="onFileChange">
      <cropper ref="cropperRef" :src="state.originalImage" :stencil-component='CircleStencil' />
      <button @click="onImageCrop">Crop</button>
    </section>

    <section v-if="state.croppedImage">
      <article>Raw Image:</article>
      <img :src="state.croppedImage" alt="Cropped Image" />
    </section>

    <section v-if="state.compressedImage">
      <article>Compressed Image:</article>
      <img :src="state.compressedImage" alt="Compressed Image">
    </section>
  </main>
</template>

<script setup lang="ts">
import { Cropper, CircleStencil } from 'vue-advanced-cropper';
import { reactive, ref } from 'vue';
import 'vue-advanced-cropper/dist/style.css';

type TState = {
  originalImage?: string,
  croppedImage?: string ,
  compressedImage?: string
}

const cropperRef = ref<typeof Cropper>();

const state: TState = reactive({})

const onFileChange = (event: Event) => {
  const { target } = event as Event & { target: EventTarget & HTMLInputElement };

  if (target?.files && target.files[0]) {
    state.originalImage = URL.createObjectURL(target.files[0]);
  }
}

const onImageCrop = () => {
  if (cropperRef.value) {
    const { canvas } = cropperRef.value.getResult();
    state.croppedImage = canvas.toDataURL();
    state.compressedImage = canvas.toDataURL('image/jpeg', 0.2);
  }
}
</script>

<style scoped></style>
