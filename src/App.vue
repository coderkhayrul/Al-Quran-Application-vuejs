<template>
  <div class="min-h-screen bg-gray-100">
    <div class="container py-6">
      <div class="bg-white p-4 shadow rounded">
        <div class="flex -mx-4">
          <div class="flex-1 px-4 items-center mb-6">
            <select @change="getSpecificSura" class="quran-input" name="" id="">
              <option value="" >Select Sura</option>
              <option :value="sura.number" v-for="sura in suras" :key="sura.number">{{ sura.name }} -{{ sura.englishName }}</option>
            </select>
          </div>
          <div class="flex-1 px-4 text-center">
            <h3 class="font-bold mb-1 text-lg">Al-Baqrah</h3>
            <p>The Cow</p>
          </div>
          <div class="flex-1 px-4">
            <select class="quran-input" name="" id="">
              <option value="">Select Ayah</option>
            </select>
          </div>
        </div>
        <div class="text-3xl">
          <p class="flex items-center mb-6" v-for="ayah in currentSura.ayahs" :key="ayah.number">
            <span class="text-xs flex items-center justify-center w-4 h-4 border rounded-full mr-4"> {{ ayah.number }} </span>
            {{ ayah.text }}
            </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      suras: [],
      currentSura: [],

    }
  },
  mounted() {

    // All Suras Get
    axios.get("https://api.alquran.cloud/v1/surah").then((res) => {
      this.suras = res.data.data;
      console.log(res.data.data);
    });

    this.querySpecificSura(1);
  },

  methods: {
    getSpecificSura(e) { 
      // console.log(e.target.value);
      this.querySpecificSura(e.target.value);
    },
    querySpecificSura(suraNumber) {
      axios.get("https://api.alquran.cloud/v1/surah/"+ suraNumber).then((res) => {
        this.currentSura = res.data.data;
      });
    }
  },
}
</script>
