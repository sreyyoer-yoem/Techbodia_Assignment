<template>
    <div class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
      <div class="bg-white p-5 rounded-lg max-w-md flex flex-col justify-center">
        <img :src="country.flags.png" alt="">
        <h2 class="text-lg font-semibold">Contry Name: {{ country.name.official }}</h2>
        <p class="text-sm text-gray-500"> <strong>cca2</strong> :{{ country.cca2 }}</p>
        <p class="text-sm text-gray-500"> <strong>cca3: </strong>{{ country.cca3 }}</p>
        <p class="text-sm text-gray-500"><strong>Status:</strong> {{ country.status }}</p>
        <p class="text-sm text-gray-500"><strong>Region: </strong>{{ country.region }}</p>
        <p class="text-sm text-gray-500"><strong>Region:</strong> {{ country.region }}</p>
        <p class="text-sm text-gray-500"><strong>startOfWeek :</strong> {{ country.startOfWeek }}</p>
        <p class="text-sm text-gray-500"><strong>languages:</strong> {{ country.languages.eng }}</p>
        <p class="text-sm text-gray-500"><strong>maps:</strong> {{ country.maps.googleMaps }}</p>
        <!-- Add other information fields here -->
        <button
          @click="closeModal"
          class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 mt-4 w-full"
        >
          Close
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      country: {
        type: Object,
        required: true
      }
    },
    methods: {
      closeModal() {
        this.$emit('closeModal');
      }
    }
  };
  </script>
  
  <style>
  /* Add Tailwind CSS classes here */
  </style>