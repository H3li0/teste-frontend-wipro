<template>
  <div id="app" class="container mt-5">
    <div class="row mb-5">
      <div class="col">
        <Grafico />
      </div>
    </div>
    <div class="row">
      <div class="col">
        <Tabela :rows="rows" />
      </div>
    </div>
  </div>
</template>

<script>
import Tabela from './components/Tabela.vue';
import Grafico from './components/Grafico.vue';
import axios from 'axios';

import {API_BASE_URL, CASES} from './utils/constants';


export default {
  name: 'App',
  components: {
    Tabela,
    Grafico
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

            for (let [nameAll, obj2] of Object.entries(obj1)) {

              if (countryName !== 'Global' && nameAll === 'All') {

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

          }
        })
        .catch(err => console.log(err));
    },
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
