<template>
  <div class="max-w-screen-xl w-full mx-auto bg-white shadow-2xl rounded-2xl grid grid-cols-3 overflow-hidden font-base">
    <section class="col-span-1 bg-gray-200 p-10">
      <div class="mb-7 space-y-2">
        <h1 class="font-bold">Hueless</h1>
        <hr class="border-b border-black">
        <h1 class="text-4xl font-bold font-display text-gray-800 tracking-wide">What Color?</h1>
      </div>
      <div class="flex flex-col space-y-5">
        <div class="grid grid-cols-2 border-2 border-gray-100 rounded-xl">
          <div class="flex flex-col p-2">
            <label for="hex" class="text-sm font-bold text-gray-700 mb-1 ml-1 uppercase">HEX</label>
            <div class="flex items-center space-x-1">
              <span class="text-2xl text-gray-600">#</span>
              <input type="text" v-model="inputHex" id="hex" class="rounded-lg px-3 py-2 w-full" placeholder="000000">
            </div>
          </div>
          <div class="flex flex-col p-2">
            <label for="rgb" class="text-sm font-bold text-gray-700 mb-1 ml-1 uppercase">RGB</label>
            <input type="text" v-model="displayRGB" id="rgb" class="rounded-lg px-3 py-2 border border-white text-gray-400" disabled>
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
        <div class="flex flex-col items-center justify-center p-2 border-2 border-gray-100 rounded-xl h-64">
          image container
        </div>
      </div>
    </section>
    <section class="col-span-2 p-10">
      <div class="flex flex-col items-center justify-center h-full">
        <p class="mb-16 text-xl">That color is a...</p>
        <div class="flex flex-col space-y-3 h-72 w-72 rounded-full items-center justify-center shadow-xl bg-gray-200" :style="`border: 7px solid #${inputHex}`">
          <span class="color-name font-display text-4xl break-words" :style="`color: #${inputHex}`">{{convertColor}}</span>
          <span class="text-gray-500 text-xs">({{convertColor}})</span>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import convert from 'color-convert';
import { hueWheel, redShades, grayShades, pinkShades } from '../utils/colorNames';

export default {
  data() {
    return {
      displayRGB: '',
      inputHex: null,
      inputColor: '',
    }
  },
  computed: {
    convertColor() {
      this.inputColor = this.inputHex;
      let sectors = hueWheel.map(item => item.sector);
      let hue = convert.hex.hsl(this.inputColor)[0];

      if(hue == 0) {
        let keyword = convert.hex.keyword(this.inputColor);
        if(redShades.includes(keyword)) {
          return 'red';
        } else if(grayShades.includes(keyword)) {
          return 'gray';
        } else if(pinkShades.includes(keyword)) {
          return 'pink';
        } else if(keyword == 'white') {
          return 'white';
        } else {
          return 'black';
        }
      } else {
        let roundedHue = sectors.reduce((prev, curr) => Math.abs(curr - hue) < Math.abs(prev - hue) ? curr : prev);
        let output = hueWheel.find(item => item.sector == roundedHue);
        return output.color
      }
    }
  },
  created() {
    if(!this.inputHex) {
      this.inputHex = '000000';
    }
  },
  watch: {
    inputHex() {
      this.displayRGB = convert.hex.rgb(this.inputHex);
    }
  },
}
</script>

<style scoped>
/* .color-name {
  -webkit-text-stroke: 1px #cbd5e0;
} */
</style>