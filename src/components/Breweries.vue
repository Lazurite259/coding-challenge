<template>
  <div class="container">
    <h1>Breweries List</h1>
    <form class="my-5">
      <div class="row my-2 justify-content-center">
        <div class="col-sm-auto">
          <label class="col-form-label">City</label>
        </div>
        <div class="col-sm-auto">
          <select v-model="city" class="form-control text-center">
            <option v-for="(option, index) in cities" :key="index">
              {{ option }}
            </option>
          </select>
        </div>
        <div class="col-sm-auto">
          <label class="col-form-label">Type</label>
        </div>
        <div class="col-sm-auto">
          <select v-model="type" class="form-control text-center">
            <option v-for="(option, index) in types" :key="index">
              {{ option }}
            </option>
          </select>
        </div>
      </div>
    </form>
    <FilteredResults :city="city" :type="type" />
  </div>
</template>

<script>
import { ref } from 'vue'
import axios from 'axios'
import FilteredResults from './FilteredResults.vue'

export default {
  components: {
    FilteredResults
  },
  setup () {
    const cities = ref(['Milwaukee'])
    const types = ref([
      'All',
      'Micro',
      'Nano',
      'Regional',
      'Brewpub',
      'Large',
      'Planning',
      'Bar',
      'Contract',
      'Proprieter',
      'Closed'
    ])
    const city = ref('Milwaukee')
    const type = ref('All')

    const citiesURL = 'https://countriesnow.space/api/v0.1/countries/cities'
    const getCities = async () => {
      await axios
        .post(citiesURL, {
          country: 'united states'
        })
        .then(res => {
          cities.value = res.data.data
        })
    }
    getCities()

    return {
      cities,
      types,
      city,
      type
    }
  }
}
</script>

<style scoped>
.form {
  display: inline-block;
  text-align: left;
  min-width: 40%;
}
.col-form-label {
  font-weight: bold;
}
</style>
