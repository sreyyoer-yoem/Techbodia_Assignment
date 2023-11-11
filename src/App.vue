<!-- 
<template>
  <h1 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white"><strong>List of Countries</strong>
  </h1>
  <div>
    <div class="div  flex justify-center mt-5">
      <select v-model="sortOrder"
        class="flex-shrink-0 z-10 inline-flex items-center py-2.5 px-4 text-sm font-medium text-center text-gray-900 bg-gray-100 border border-gray-300 rounded-l-lg hover:bg-gray-200 focus:ring-4 focus:outline-none focus:ring-gray-100 dark:bg-gray-700 dark:hover:bg-gray-600 dark:focus:ring-gray-700 dark:text-white dark:border-gray-600">
        <option value="asc">Sort by Country Name (Ascending)</option>
        <option value="desc">Sort by Country Name (Descending)</option>
      </select>
      <div class="relative w-96 ">
        <input type="search" id="search-dropdown"
          class="block p-2.5 w-full  text-sm text-gray-900 bg-gray-50 rounded-r-lg border-l-gray-50 border-l-2 border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-l-gray-700  dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:border-blue-500"
          v-model="searchContryName" placeholder="Search by Country Name">
        <button type="submit"
          class="absolute top-0 right-0 p-2.5 text-sm font-medium h-full text-white bg-blue-700 rounded-r-lg border border-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
          <svg class="w-4 h-4" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z" />
          </svg>
          <span class="sr-only">Search</span>
        </button>

      </div>

    </div>
    <div class="div flex flex-wrap justify-center p-5">
      <div v-for="item in countryLists" :key="item.cca2"
        class="  m-4 max-w-sm bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700">
        <a href="#" class="flex justify-center items-center mt-4">
          <img class="object-cover w-full rounded-t-lg h-96 md:h-auto md:w-48 md:rounded-none md:rounded-l-lg"
            :src="item.flags.png" alt="Country Flag" />
        </a>
        <div class=" info p-5 flex flex-col items-start">
          <a href="#">
            <h5 @click="openPopup(item)"
              class="cursor-pointer mb-2 text-xl font-bold tracking-tight text-gray-900 dark:text-white"><strong>Contry
                Name:</strong> {{ item.name.official }}</h5>
          </a>
          <span><strong>cca2: </strong>{{ item.cca2 }}</span>
          <span><strong>cca3: </strong>{{ item.cca3 }}</span>
          <span><strong>NativeName: </strong>{{ item.name.nativeName }}</span>
          <span><strong>AltSpellinngs: </strong>{{ item.altSpellings }}</span>
          <span><strong>idd:</strong>{{ item.idd.root }}{{ item.idd.suffixes }}</span>

        </div>
      </div>
    </div>

    
  
    <div v-if="selectedCountry" class="fixed inset-10 flex items-center justify-center z-50">
      <div class=" absolute   opacity-75"></div>
      <div class=" inset-5 w-full bg-white border border-gray-200 rounded-lg shadow-lg p-5 max-w-md">
       
        <a href="#" class="flex justify-center items-center mt-4">
          <img class="object-cover w-full rounded-t-lg h-100 md:h-auto md:w-48 md:rounded-none md:rounded-l-lg"
            :src="selectedCountry.flags.png" alt="Country Flag" />
        </a>
        <h2 class="text-xl m-4">{{ selectedCountry.name.official }}</h2>
        <p><strong>Region:</strong> {{ selectedCountry.region }}</p>
        <p> <strong>Status:</strong> {{ selectedCountry.status }}</p>
        <p><strong>Subregion:</strong> {{ selectedCountry.subregion }}</p>
        <p><strong>Area:</strong> {{ selectedCountry.area }}</p>
        <p><strong>Flag:</strong> {{ selectedCountry.flag }}</p>
        <p><strong>Population:</strong> {{ selectedCountry.population }}</p>
       
        <button class="w-full mt-4 py-2 px-4 bg-blue-500 text-white rounded-lg" @click="selectedCountry = null">
          Close
        </button>
      </div>
    </div>

 
    <div class="pagination">
      <button @click="previousPage">Previous</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button @click="nextPage">Next</button>
    </div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      countryLists: [],
      searchContryName: "",
      sortOrder: "asc",
      selectedCountry: null,
      currentPage: 1,
      perPage: 25,
    };
  },
  mounted() {
    this.listContrys();

  },
  computed: {
    filteredCountries() {
      let filtered = this.countryLists.filter((country) => {
        return country.name.official.toLowerCase().includes(this.searchContryName.toLowerCase());
      });

      if (this.sortOrder === "asc") {
        alert("1")
        filtered.sort((a, b) => (a.name.official > b.name.official ? 1 : -1));
      } else if (this.sortOrder === "desc") {
        alert("2")
        filtered.sort((a, b) => (a.name.official < b.name.official ? 1 : -1));
      }

      const startIndex = (this.currentPage - 1) * this.perPage;
      const endIndex = startIndex + this.perPage;
      return filtered.slice(startIndex, endIndex);

    },

  },

  methods: {
    listContrys() {
      axios
        .get("https://restcountries.com/v3.1/all")
        .then((response) => {
          this.countryLists = response.data;
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
    goToPage(page) {
      this.currentPage = page;
    },

    nextPage() {
      alert("hello");
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },

    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    openPopup(country) {
      this.selectedCountry = country;
    },

  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style> 
 -->

 <template>
  <div>
    <h1 class="text-center text-2xl mt-10"><strong>List of Contry</strong></h1>
    <CountryCatalog />
  </div>
</template>

<script>
import CountryCatalog from './components/CountryCatalog.vue';

export default {
  components: {
    CountryCatalog
  }
};
</script>