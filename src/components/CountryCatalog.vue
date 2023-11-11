<template>
    <div>
      <div class="div flex justify-center mt-5">
        <select v-model="sortOrder" class="flex-shrink-0 inline-flex items-center py-2.5 px-4 text-sm font-medium text-center text-gray-900 bg-gray-100 border border-gray-300 rounded-l-lg hover:bg-gray-200 focus:ring-4 focus:outline-none focus:ring-gray-100 dark:bg-gray-700 dark:hover:bg-gray-600 dark:focus:ring-gray-700 dark:text-white dark:border-gray-600">
          <option value="asc">Sort by Country Name (Ascending)</option>
          <option value="desc">Sort by Country Name (Descending)</option>
        </select>
        <div class="relative w-96">
          <input type="search" id="search-dropdown" class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-r-lg border-l-gray-50 border-l-2 border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-l-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:border-blue-500" v-model="searchTerm" @input="searchCountries" placeholder="Search by Country Name">
          <button type="submit" class="absolute top-0 right-0 p-2.5 text-sm font-medium h-full text-white bg-blue-700 rounded-r-lg border border-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
            <svg class="w-4 h-4" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z" />
            </svg>
            <span class="sr-only">Search</span>
          </button>
        </div>
      </div>
  
      <!-- Country list -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-5">
        <CountryCard v-for="country in paginatedCountries" :key="country.cca3" :country="country" @countryClick="showCountryDetails" />
      </div>
  
      <!-- Pagination -->
      <div class="flex justify-center m-4">
        <button @click="previousPage" :disabled="currentPage === 1" class="rounded-lg bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 mr-2">
          Previous Page
        </button>
        <button @click="nextPage" :disabled="currentPage === totalPages" class="rounded-lg bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4">
          Next Page
        </button>
      </div>
  
      <!-- Country details modal -->
      <CountryModal v-if="selectedCountry" :country="selectedCountry" @closeModal="closeCountryDetails" />
    </div>
  </template>
  
  <script>
  import CountryCard from './CountryCard.vue';
  import CountryModal from './CountryModal.vue';
  
  export default {
    components: {
      CountryCard,
      CountryModal
    },
    data() {
      return {
        countries: [], // Array to store all countries
        searchTerm: '', // Search term
        sortOrder: 'asc', // Sorting order
        currentPage: 1, // Current page
        countriesPerPage: 25, // Number of countries per page
        selectedCountry: null // Selected country for modal
      };
    },
    computed: {
      filteredCountries() {
        // Filter countries based on search term
        const filtered = this.countries.filter(country =>
          country.name.official.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
  
        // Sort countries based on sorting order
        const sorted = filtered.sort((a, b) => {
          const nameA = a.name.official.toLowerCase();
          const nameB = b.name.official.toLowerCase();
          return this.sortOrder === 'asc' ? nameA.localeCompare(nameB) : nameB.localeCompare(nameA);
        });
  
        return sorted;
      },
      totalPages() {
        // Calculate total number of pages
        return Math.ceil(this.filteredCountries.length / this.countriesPerPage);
      },
      paginatedCountries() {
        // Paginate countries based on current page
        const startIndex = (this.currentPage - 1) * this.countriesPerPage;
        const endIndex = startIndex + this.countriesPerPage;
        return this.filteredCountries.slice(startIndex, endIndex);
      }
    },
    methods: {
      async fetchCountries() {
        // Fetch countries data from the API
        const response = await fetch('https://restcountries.com/v3.1/all');
        const data = await response.json();
        this.countries = data;
      },
      searchCountries() {
        // Update search term and reset current page to 1
        this.currentPage = 1;
      },
      previousPage() {
        // Go to previous page
        this.currentPage--;
      },
      nextPage() {
        // Go to next page
        this.currentPage++;
      },
      showCountryDetails(country) {
        // Show country details modal
        this.selectedCountry = country;
      },
      closeCountryDetails() {
        // Close country details modal
        this.selectedCountry = null;
      }
    },
    mounted() {
      // Fetch countries data when the component is mounted
      this.fetchCountries();
    }
  };
  </script>
  
  <style>
  /* Add Tailwind CSS classes here */
  </style>