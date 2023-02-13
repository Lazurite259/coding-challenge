<template>
  <div class="my-5">
    <table class="table table-striped">
      <thead class="thead-dark">
        <tr>
          <th scope="col">Name</th>
          <th scope="col">Brewery Type</th>
          <th scope="col">Street</th>
          <th scope="col">Phone</th>
          <th scope="col">Website URL</th>
        </tr>
      </thead>
      <tbody>
        <tr scope="row" v-for="(brewery, index) in breweries" :key="index">
          <td class="font-bold">{{ brewery.name }}</td>
          <td class="text-capitalize">{{ brewery.brewery_type }}</td>
          <td>{{ brewery.street }}</td>
          <td>{{ formatPhoneNumber(brewery.phone) }}</td>
          <td>
            <a class="text-link" :href="brewery.website_url" target="_blank">{{
              brewery.website_url
            }}</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { watch, ref } from 'vue'
import axios from 'axios'
export default {
  props: {
    city: String,
    type: String
  },
  setup (props) {
    const breweries = ref([])

    const getBreweries = async () => {
      let breweriesURL = 'https://api.openbrewerydb.org/breweries'

      breweriesURL += `?by_city=${props.city.replace(' ', '_')}`
      if (props.type !== 'All') {
        breweriesURL += `&by_type=${props.type.toLowerCase()}`
      }

      await axios.get(breweriesURL).then(res => {
        breweries.value = res.data
      })
    }
    getBreweries()

    watch(
      () => [props.city, props.type],
      () => {
        getBreweries()
      }
    )

    const formatPhoneNumber = phoneNumberString => {
      const cleaned = ('' + phoneNumberString).replace(/\D/g, '')
      const match = cleaned.match(/^(1|)?(\d{3})(\d{3})(\d{4})$/)
      if (match) {
        const intlCode = match[1] ? '+1 ' : ''
        return [intlCode, '(', match[2], ') ', match[3], '-', match[4]].join('')
      }
      return null
    }

    return {
      breweries,
      formatPhoneNumber
    }
  }
}
</script>

<style scoped>
.table {
  text-align: left;
}
.font-bold {
  font-weight: 500;
}
.text-link {
  text-decoration: none;
}
</style>
