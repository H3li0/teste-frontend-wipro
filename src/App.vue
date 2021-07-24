<template>
  <div id="app" class="container mt-5">
    <Tabela :rows="rows" />
  </div>
</template>

<script>
import Tabela from './components/Tabela.vue';
import axios from 'axios';

import {API_BASE_URL, CASES} from './utils';


export default {
  name: 'App',
  components: {
    Tabela
  },
  data: function() {
    return {
      rows: [],
    }
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData: function() {

      let country;
      let countries = [];
      let idValue = 0;

      axios.get(`${API_BASE_URL}${CASES}`)
        .then(response => {
          for (let [countryName, obj1] of Object.entries(response.data)) {

            // if (countryName !== 'Global') {

              for (let [nameAll, obj2] of Object.entries(obj1)) {

                if (nameAll === 'All') {

                  country = {
                    id: ++idValue,
                    pais: countryName,
                    confirmados: obj2.confirmed,
                    recuperados: obj2.recovered,
                    mortes: obj2.deaths,
                  };

                  countries.push(country);
                }

              }
              this.rows = countries;
            // }

          }
        })
        .catch(err => console.log(err));
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
