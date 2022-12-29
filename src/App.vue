<template>
<div class="min-h-screen bg-gray-100">
  <div class="container py-6">
    <div class="bg-white p-4 shadow rounded">
      <div class="flex -mx-4 items-center mb-6">
        <div class="flex-1 px-4">
          <select @change="getSpecificSura" class="quran-input" name="" id="">
            <option value="">Select Sura</option>
            <option v-for="sura in suras"
                    :value="sura.number">{{sura.name}}-{{sura.englishName
              }}</option>
          </select>
        </div>
        <div class="flex-1 px-4 text-center">
          <h3 class="font-bold mb-1 text-lg">
            {{ currentSura.name }}
          </h3>
          <p>{{ currentSura.englishName }}</p>
        </div>
      </div>
      <div class="flex">
        <div class="flex-1">
          <div v-if="loading" class="flex">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none"
                 viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
                 class="w-6 h-6 animate-spin">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9.594 3.94c.09-.542.56-.94 1.11-.94h2.593c.55 0 1.02.398 1.11.94l.213 1.281c.063.374.313.686.645.87.074.04.147.083.22.127.324.196.72.257 1.075.124l1.217-.456a1.125 1.125 0 011.37.49l1.296 2.247a1.125 1.125 0 01-.26 1.431l-1.003.827c-.293.24-.438.613-.431.992a6.759 6.759 0 010 .255c-.007.378.138.75.43.99l1.005.828c.424.35.534.954.26 1.43l-1.298 2.247a1.125 1.125 0 01-1.369.491l-1.217-.456c-.355-.133-.75-.072-1.076.124a6.57 6.57 0 01-.22.128c-.331.183-.581.495-.644.869l-.213 1.28c-.09.543-.56.941-1.11.941h-2.594c-.55 0-1.02-.398-1.11-.94l-.213-1.281c-.062-.374-.312-.686-.644-.87a6.52 6.52 0 01-.22-.127c-.325-.196-.72-.257-1.076-.124l-1.217.456a1.125 1.125 0 01-1.369-.49l-1.297-2.247a1.125 1.125 0 01.26-1.431l1.004-.827c.292-.24.437-.613.43-.992a6.932 6.932 0 010-.255c.007-.378-.138-.75-.43-.99l-1.004-.828a1.125 1.125 0 01-.26-1.43l1.297-2.247a1.125 1.125 0 011.37-.491l1.216.456c.356.133.751.072 1.076-.124.072-.044.146-.087.22-.128.332-.183.582-.495.644-.869l.214-1.281z" />
              <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
            </svg>
            loading...
          </div>

          <div v-else class="text-4xl">
            <p class="flex items-center mb-6"
               v-if="currentSura.hasOwnProperty('ayahs')" v-for="ayah in
           currentSura.ayahs">
          <span
              class="text-xs inline-block w-5 h-5 flex items-center justify-center border rounded-full mr-4">
            {{ayah.numberInSurah}}
          </span>
              {{ayah.text}}
            </p>
          </div>
        </div>
        <div class="flex-1">
          <p class="flex items-center" v-if="currentSurahBangla.hasOwnProperty('ayahs')" v-for="ayah in currentSurahBangla.ayahs"><span class="text-xs inline-block mr-4 w-5 h-5 flex items-center justify-center border rounded-full">{{ ayah.numberInSurah }}</span>
            <span class="flex-1">
                <span>
                  {{ ayah.text }}
                </span><br>

              </span>
          </p>
        </div>
        </div>



    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  data () {
    return {
      suras: [],
      currentSura: [],
      loading: true,
      currentSurahBangla: []
    }
  },
  mounted() {
    axios.get('https://api.alquran.cloud/v1/surah').then(response => {
      this.suras = response.data.data
    })
    this.querySpecificSura(1);

  },
  methods: {
    getSpecificSura(e) {
      this.querySpecificSura(e.target.value);
    },
    querySpecificSura(suraNumber) {
      this.loading = true;
      axios.get('https://api.alquran.cloud/v1/surah/' + suraNumber).then(response => {
        this.currentSura = response.data.data;
        this.loading =false;
      });
      axios.get('https://api.alquran.cloud/v1/surah/'+suraNumber+'/editions/bn.bengali').then(response => {
        // console.log(response.data.data[0])
        this.currentSurahBangla = response.data.data[0];
      })

    }

  }
}
</script>