<template>
  <div class="max-w-screen-xl w-full mx-auto relative bg-white shadow-2xl rounded-2xl grid grid-cols-1 md:grid-cols-3 overflow-hidden font-base">
    <section class="col-span-1 bg-gray-200 p-10">
      <div class="mb-7 space-y-2">
        <h1 class="font-bold">Hueless</h1>
        <hr class="border-b border-black">
        <h1 class="text-4xl font-bold font-display tracking-wide">What Color?</h1>
        <p class="text-sm text-gray-700">Provide a hex color code or pick a color from an uploaded image.</p>
        <p class="text-sm md:hidden text-gray-700">Then swipe going down to see the color name.</p>
      </div>
      <div class="flex flex-col space-y-5 text-gray-700">
        <!-- input group 1 (hex) -->
        <div class="grid grid-cols-2 border-2 border-gray-300 rounded-xl">
          <!-- hex input -->
          <div class="flex flex-col p-2">
            <label for="hex" class="text-sm font-bold mb-3 ml-1 uppercase">HEX</label>
            <div class="flex items-center space-x-1">
              <span class="text-2xl text-gray-600">#</span>
              <input type="text" v-model="inputHex" id="hex" class="rounded-lg px-3 py-2 w-full" placeholder="000000">
            </div>
          </div>
          <!-- rgb equivalent -->
          <div class="flex flex-col p-2">
            <label for="rgb" class="text-sm font-bold mb-3 ml-1 uppercase">RGB</label>
            <input type="text" v-model="displayRGB" id="rgb" class="rounded-lg px-3 py-2 border border-white text-gray-400" disabled>
          </div>
        </div>
        <!-- end input group -->

        <!-- input group 2 (image) -->
        <div class="flex flex-col border-2 border-gray-300 rounded-xl p-2 space-y-2">
          <!-- upload image -->
          <div class="flex flex-col p-2 border-2 border-gray-100 rounded-xl">
            <label for="file" class="text-sm font-bold mb-3 ml-1 uppercase">Upload image</label>
            <input type="file" id="file" ref="file" @change="handleUpload" class="rounded-lg px-3 focus:outline-none">
          </div>
          <!-- image color picker -->
          <div class="flex flex-col items-center justify-center p-2 border-2 border-gray-100 rounded-xl h-56 overflow-hidden">
            <img v-if="image" :src="image" alt="uploaded image" class="h-full object-contain">
          </div>
          <button @click="expandImage = true" v-if="image"
            class="py-1 uppercase text-xs text-white rounded shadow-lg bg-blue-400 hover:bg-blue-500 focus:outline-none">
              expand image
          </button>
        </div>
        <!-- end input group -->
      </div>
    </section>

    <section class="col-span-1 md:col-span-2 p-10">
      <div class="flex flex-col items-center justify-center h-full">
        <p class="mb-16 text-xl">That color is a...</p>
        <div class="flex flex-col space-y-3 h-72 w-72 rounded-full items-center justify-center shadow-xl bg-gray-200" :style="`border: 7px solid #${inputHex}`">
          <span class="color-name font-display text-4xl break-words" :style="`color: #${inputHex}`">{{convertColor}}</span>
          <span class="text-gray-500 text-xs">({{convertColor}})</span>
        </div>
      </div>
    </section>
  </div>

  <!-- expanded image modal -->
  <div v-show="expandImage"
    class="absolute top-0 left-0 bg-gray-500 bg-opacity-60 flex flex-col justify-center items-center w-full h-full p-5 overscroll-none">
    <div class="flex w-full justify-end">
      <button @click="expandImage = false"
      class="text-white hover:text-gray-300 focus:outline-none">
        <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-square-x" width="44" height="44" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
          <rect x="4" y="4" width="16" height="16" rx="2" />
          <path d="M10 10l4 4m0 -4l-4 4" />
        </svg>
      </button>
    </div>
    <section class="relative bg-white w-11/12 h-full md:flex items-center justify-center overflow-hidden rounded-2xl">
      <div class="hidden md:block absolute top-0 right-0 m-2 z-10">
        <div class="flex flex-col space-y-3 h-24 w-24 2xl:h-36 2xl:w-36 rounded-full items-center justify-center bg-gray-200" :style="`border: 7px solid #${inputHex}`">
          <span class="color-name font-display text-xl break-words" :style="`color: #${inputHex}`">{{convertColor}}</span>
          <span class="text-gray-500 text-xs">({{convertColor}})</span>
        </div>
      </div>
      <div class="max-w-xl md:max-w-screen-sm lg:max-w-screen-md xl:max-w-screen-lg 2xl:max-w-screen-xl max-h-full mx-auto overflow-auto">
        <ImgColorPicker
          v-if="image"
          :key="pickerKey"
          :width="imageWidth"
          :showColor="false"
          @setColor="setColor"
          :imagesrc="image"
          class="overflow-auto">
        </ImgColorPicker>
      </div>
    </section>
  </div>
  <!-- end modal -->
</template>

<script>
import convert from 'color-convert';
import { hueWheel, redShades, grayShades, pinkShades } from '../utils/colorNames';
import ImgColorPicker from 'vue-img-color-picker';

export default {
  components: {
    ImgColorPicker,
  },
  data() {
    return {
      displayRGB: '',
      inputHex: null,
      inputColor: '',
      image: null,
      imageWidth: null,
      expandImage: false,
      pickerKey: 0,
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
  methods: {
    handleUpload(event) {
      let file = this.$refs.file.files[0];

      let reader = new FileReader();
      reader.onload = () => {
        this.image = reader.result;

        let imageData = new Image();
        imageData.src = reader.result;
        imageData.onload = () => {
          this.imageWidth = imageData.width
        }
      }

      reader.readAsDataURL(file);
    },
    setColor(color) {
      this.inputHex = color.substring(1);
    },
    pickerRender() {
      this.pickerKey += 1;
    }
  },
  watch: {
    inputHex() {
      this.displayRGB = convert.hex.rgb(this.inputHex);
    },
    image() {
      this.pickerRender();
    }
  },
}
</script>

<style scoped>
input[type=file]::-webkit-file-upload-button {
  @apply px-5 py-3 rounded-xl shadow-lg bg-blue-400 text-white border-0;
  cursor: pointer;
}

input[type=file]::-webkit-file-upload-button:focus {
  outline: none;
}

input[type=file]::-webkit-file-upload-button:hover {
  @apply bg-blue-500;
}
</style>