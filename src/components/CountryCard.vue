<template>
  <div class="div flex flex-wrap justify-center p-5">
    <div
      
      class="max-w-sm bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700"
    >
      <a href="#" class="flex justify-center items-center mt-4">
        <img
          class="object-cover w-full rounded-t-lg h-96 md:h-auto md:w-48 md:rounded-none md:rounded-l-lg"
          :src="country.flags.png"
          alt="Country Flag"
        />
      </a>
      <div class="info p-5 flex flex-col items-start">
        <a href="#">
          <h5
          @click="openModal" class="cursor-pointer mb-2 text-xl font-bold tracking-tight text-gray-900 dark:text-white"
          >
            <strong>Contry Name:</strong> {{ country.name.official }}
          </h5>
        </a>
        <span><strong>cca2: </strong>{{ country.cca2 }}</span>
        <span><strong>cca3: </strong>{{ country.cca3 }}</span>
        <span><strong>NativeName: </strong>{{ country.name.nativeName }}</span>
        <!-- <span><strong>AltSpellinngs: </strong>{{ country.altSpellings }}</span> -->
        <div>
          <span><strong>AltSpellinngs: </strong></span>
          <ul v-for="altspelling in country.altSpellings" :key="altspelling">
            <li>- {{ altspelling }},</li>
          </ul>
        </div>

        <div>
          <!-- <div>
            <span><strong>AltSpellinngs:</strong>{{ nan() }}</span>
          </div> -->
          <div>
            <span><strong>idd:</strong>{{ idd() }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    country: {
      type: Object,
      required: true,
    },
  },
  methods: {
    openModal() {
      this.$emit("countryClick", this.country);
    },
    idd() {
      if (
        !this.country ||
        !this.country.idd ||
        !this.country.idd.suffixes ||
        !Array.isArray(this.country.idd.suffixes)
      ) {
        return ""; // Handle the case when data is missing or not properly structured
      }
      const root = this.country.idd.root || "";
      const suffixesSum = this.country.idd.suffixes
        .map((suffix) => Number(suffix))
        .filter((suffixValue) => !isNaN(suffixValue))
        .reduce((accumulator, suffixValue) => accumulator + suffixValue, 0);

      return root + suffixesSum;
    },
    altspelling(){
      // let altspelling = "";
      // for(let n in this.country.altspelling){
      //   let altspelling = n.
      //   console.log(n);
      // }
      // return altspelling;

    }
  },
};
</script>

<style>
/* Add Tailwind CSS classes here */
</style>
