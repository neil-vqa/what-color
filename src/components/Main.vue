<template>
  <div class="max-w-screen-xl w-full mx-auto bg-white shadow-2xl rounded-2xl grid grid-cols-3 overflow-hidden">
    <section class="col-span-1 bg-gray-200 p-10">
      <div class="mb-7">
        <h1 class="text-5xl font-bold">What Color?</h1>
      </div>
      <div class="flex flex-col space-y-5">
        <div class="grid grid-cols-2 border-2 border-gray-100 rounded-xl">
          <div class="flex flex-col p-2">
            <label for="hex" class="text-sm font-bold text-gray-700 mb-1 ml-1 uppercase">HEX</label>
            <input type="text" v-model="inputHex" id="hex" class="rounded-lg px-3 py-2">
          </div>
          <div class="flex flex-col p-2">
            <label for="rgb" class="text-sm font-bold text-gray-700 mb-1 ml-1 uppercase">RGB</label>
            <input type="text" v-model="inputRGB" id="rgb" class="rounded-lg px-3 py-2">
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
      That color is a... {{convertColor}}
    </section>
  </div>
</template>

<script>
import convert from 'color-convert';

import Navbar from './Navbar.vue'

export default {
  components: {
    Navbar
  },
  data() {
    return {
      inputRGB: '',
      inputHex: '',
      inputColor: '',
      outputName: '',
      hueWheel: [
        {sector: 0, color: 'red'},
        {sector: 30, color: 'orange'},
        {sector: 50, color: 'yellow/gold'},
        {sector: 60, color: 'yellow'},
        {sector: 75, color: 'yellow-green'},
        {sector: 120, color: 'green'},
        {sector: 160, color: 'green'},
        {sector: 180, color: 'cyan/blue-green'},
        {sector: 197, color: 'blue'},
        {sector: 240, color: 'blue'},
        {sector: 270, color: 'violet'},
        {sector: 285, color: 'purple'},
        {sector: 300, color: 'magenta/fuchsia'},
        {sector: 330, color: 'rose'},
        {sector: 350, color: 'pink'},
        {sector: 360, color: 'red'},
      ],
      redShades: [
        'red',
        'lightsalmon',
        'lightcoral',
        'salmon',
        'darksalmon',
        'tomato',
        'indianred',
        'orangered',
        'crimson',
        'firebrick',
        'darkred',
        'maroon'
      ],
      grayShades: [
        'grey',
        'gainsboro',
        'lightgrey',
        'silver',
        'darkgrey',
        'lightslategrey',
        'slategrey',
        'dimgrey',
        'ghostwhite',
        'whitesmoke',
      ],
      pinkShades: [
        'pink',
        'mistyrose',
        'lightpink',
        'hotpink',
        'palevioletred',
        'deeppink'
      ],
    }
  },
  computed: {
    convertColor() {
      this.inputColor = this.inputHex;
      let sectors = this.hueWheel.map(item => item.sector);
      let hue = convert.hex.hsl(this.inputColor)[0];

      if(hue == 0) {
        let keyword = convert.hex.keyword(this.inputColor);
        if(this.redShades.includes(keyword)) {
          return 'red';
        } else if(this.grayShades.includes(keyword)) {
          return 'gray';
        } else if(this.pinkShades.includes(keyword)) {
          return 'pink';
        } else if(keyword == 'white') {
          return 'white';
        } else {
          return 'black';
        }
      } else {
        let roundedHue = sectors.reduce((prev, curr) => Math.abs(curr - hue) < Math.abs(prev - hue) ? curr : prev);
        let output = this.hueWheel.find(item => item.sector == roundedHue);
        return output.color
      }
    }
  },
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