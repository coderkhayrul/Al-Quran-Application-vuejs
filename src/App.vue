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

            <h3 class="font-bold mb-1 text-lg">{{ currentSura.name }}</h3>
            <p>{{ currentSura.englishName }}</p>
          </div>
        </div>

        <!-- PlaceHolder Start-->
        <div v-if="loading" class="flex">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" strokeWidth={1.5} stroke="currentColor"
            class="w-6 h-6 animate-spin">
            <path strokeLinecap="round" strokeLinejoin="round"
              d="M4.5 12a7.5 7.5 0 0015 0m-15 0a7.5 7.5 0 1115 0m-15 0H3m16.5 0H21m-1.5 0H12m-8.457 3.077l1.41-.513m14.095-5.13l1.41-.513M5.106 17.785l1.15-.964m11.49-9.642l1.149-.964M7.501 19.795l.75-1.3m7.5-12.99l.75-1.3m-6.063 16.658l.26-1.477m2.605-14.772l.26-1.477m0 17.726l-.26-1.477M10.698 4.614l-.26-1.477M16.5 19.794l-.75-1.299M7.5 4.205L12 12m6.894 5.785l-1.149-.964M6.256 7.178l-1.15-.964m15.352 8.864l-1.41-.513M4.954 9.435l-1.41-.514M12.002 12l-3.75 6.495" />
          </svg>
          Loading...
        </div>
        <!-- PlaceHolder End-->

        <div v-else class="text-3xl">
          <p class="flex items-center mb-6" v-for="ayah in currentSura.ayahs" :key="ayah.number">
            <span class="text-xs flex items-center justify-center w-4 h-4 border rounded-full mr-4"> {{ ayah.numberInSurah }} </span>
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
      loading: true,

    }
  },
  mounted() {
    // All Suras Get
    axios.get("https://api.alquran.cloud/v1/surah").then((res) => {
      this.suras = res.data.data;
    });
    this.querySpecificSura(1);
  },

  methods: {
    getSpecificSura(e) { 
      // console.log(e.target.value);
      this.querySpecificSura(e.target.value);
    },
    querySpecificSura(suraNumber) {
      this.loading = true;
      axios.get("https://api.alquran.cloud/v1/surah/" + suraNumber).then((res) => {
        // console.log(res.data.data);
        this.currentSura = res.data.data;
        this.loading = false;
      });
    }
  },
}
</script>
