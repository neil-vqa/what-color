<template>
  <div class="max-w-screen-xl w-full mx-auto bg-white shadow-2xl rounded-2xl grid grid-cols-3 overflow-hidden">
    <section class="col-span-1 bg-gray-200 p-10">
      <div class="mb-6">
        <h1 class="text-5xl font-bold">What Color?</h1>
        <h2 class="text-xl">by Neil Van</h2>
      </div>
      <div class="flex flex-col space-y-5">
        <div class="grid grid-cols-2 border-2 border-gray-100 rounded-xl">
          <div class="flex flex-col p-2">
            <label for="hex" class="text-sm font-bold text-gray-700 mb-1 ml-1 uppercase">HEX</label>
            <input type="text" id="hex" class="rounded-lg px-3 py-2">
          </div>
          <div class="flex flex-col p-2">
            <label for="rgb" class="text-sm font-bold text-gray-700 mb-1 ml-1 uppercase">RGB</label>
            <input type="text" id="rgb" class="rounded-lg px-3 py-2">
          </div>
        </div>
        <div class="flex flex-col p-2 border-2 border-gray-100 rounded-xl">
          <label for="url" class="text-sm font-bold text-gray-700 mb-1 ml-1 uppercase">URL</label>
          <input type="text" id="url" class="rounded-lg px-3 py-2">
          <button class="px-5 py-3 mt-4 rounded-xl shadow-lg bg-blue-400 text-white focus:outline-none hover:bg-blue-500">Take Screenshot</button>
        </div>
        <div class="flex flex-col p-2 border-2 border-gray-100 rounded-xl">
          <label for="file" class="text-sm font-bold text-gray-700 mb-1 ml-1 uppercase">Select image</label>
          <input type="file" id="file" class="rounded-lg px-3 py-2">
          <button class="px-5 py-3 mt-4 rounded-xl shadow-lg bg-blue-400 text-white focus:outline-none hover:bg-blue-500">Upload</button>
        </div>
        <div class="flex flex-col p-2 border-2 border-gray-100 rounded-xl h-64">
          image placeholder
        </div>
      </div>
    </section>
    <section class="col-span-2 p-10">
      That color is a...
    </section>
  </div>
</template>

<script>
import Navbar from './Navbar.vue'
export default {
  components: {
    Navbar
  }
}
</script>

<style>
body {
  background-color: aqua;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

</style>