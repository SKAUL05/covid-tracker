<template>
    <div class="flex flex-wrap">
    <div class="w-full">
      <ul class="flex mb-0 list-none flex-wrap pt-3 pb-4 flex-row">
        <li class="-mb-px mr-2 last:mr-0 flex-auto text-center">
          <a class="text-xs font-bold uppercase px-5 py-3 shadow-lg rounded block leading-normal" v-on:click="toggleTabs(1)" v-bind:class="{'text-black-600 bg-white': openTab !== 1, 'text-white bg-blue-600': openTab === 1}">
            <i class="fas fa-chart-area"></i> Stats
          </a>
        </li>
        <li class="-mb-px mr-2 last:mr-0 flex-auto text-center">
          <a class="text-xs font-bold uppercase px-5 py-3 shadow-lg rounded block leading-normal" v-on:click="toggleTabs(2)" v-bind:class="{'text-black-600 bg-white': openTab !== 2, 'text-white bg-blue-600': openTab === 2}">
            <i class="fas fa-newspaper"></i> News
          </a>
        </li>
      </ul>
      <div class="relative flex flex-col min-w-0 break-words bg-white w-full mb-6 shadow-lg rounded">
        <div class="px-4 py-5 flex-auto">
          <div class="tab-content tab-space">
            <div v-bind:class="{'hidden': openTab !== 1, 'block': openTab === 1}">
              <main v-if="!loading">
                  <DataTitle :text="title" :dataDate="dataDate"/>
                  <DataBoxes :stats="stats" />
                  <CountrySelect @get-country="getCountryData" :countries="countries"/>
                  <button @click="clearCountryData" v-if="stats.Country" class="bg-green-700 text-white roounded p-3 mt-8 focus:outline-none hover:bg-green-600 mb-2" >
                    Clear Country
                  </button> <br/>
                  <VaccineTitle :text="vaccTitle" :dataDate="vaccDate" />
                  <VaccineBoxes :stats="vaccStats" @get-vaccine-country="getVaccineCountry" :countries="vaccCountries" />
              </main>
              <main class="flex flex-col align-center justify-center text-center" v-else>
                <div class="text-gray-500 text-3xl mt-10 mb-6">
                  Fetching Data
                </div>
                <img :src="loadingImage" class="w-24 m-auto" alt="" />

              </main>
            </div>
            <div v-bind:class="{'hidden': openTab !== 2, 'block': openTab === 2}">
                <News :articles="articles" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
// @ is an alias to /src
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'
import CountrySelect from '@/components/CountrySelect'
import News from '@/components/News'
import VaccineTitle from '@/components/VaccineTitle'
import VaccineBoxes from '@/components/VaccineBoxes'

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
    News,
    VaccineTitle,
    VaccineBoxes
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      vaccTitle:'India',
      vaccDate:'',
      dataDate: '',
      stats: {},
      vaccStats: {},
      countries: [],
      vaccCountries: [],
      articles: [],
      loadingImage: require('../assets/hourglass.gif'),
      openTab: 1
    }
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://cors.bridged.cc/https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },
    async fetchNews() {
      console.log(process.env.API_KEY)
      const res = await fetch('https://cors.bridged.cc/https://newsdata.io/api/1/news?apikey=pub_166fdb99a98ee367ed171eabd118b4e7175&q=COVID&language=en')
      const data = await res.json()
     return data

    },
    async fetchVaccData(){
      const vacc_data = await fetch('https://covid.ourworldindata.org/data/owid-covid-data.json')
      const vacc = await vacc_data.json()
      return vacc
    },
    getCountryData(country) {
      this.stats = country
      this.title = country.Country
    },
    getVaccineCountry(country){
      console.log(country)
      this.vaccTitle = country.location
      this.vaccStats = country.data[country.data.length-1]
    },
    async clearCountryData() {
      this.loading = true
      const data = await this.fetchCovidData()
      this.title = "Global"
      this.stats = data.Global
      this.loading = false
},
 toggleTabs: function(tabNumber){
      this.openTab = tabNumber
    }
  },
  async created(){
    console.log("Created.....")
    const data = await this.fetchCovidData()
    console.log(data)
    const newsData = await this.fetchNews()
    console.log(newsData)
    this.articles = newsData.results
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    const vaccData = await this.fetchVaccData()
    console.log(vaccData)
    this.vaccTitle = vaccData.IND.location
    this.vaccDate = vaccData.IND.data[vaccData.IND.data.length-1].date
    this.vaccStats = vaccData.IND.data[vaccData.IND.data.length-1]
    this.vaccCountries = vaccData
    this.loading = false

  }
}
</script>
