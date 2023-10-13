<template>
  <div class="hello">
    <!-- Combobox con v-for-->
    <div class="mb-3">
        <label for="paises" class="form-label">PAISES</label>
        <div class="input-group">
          <select v-model="paisSeleccionado" class="form-select" id="paises" @change="cargarEstado">
            <option value="" hidden>Seleccione un pais</option>
            <option v-for="pais in paises" :key="pais.id" :value="pais.id">{{pais.name}}</option>
          </select>
        </div>
      </div>
  </div>

  <!-- Combobox 2 con v-for-->
  <div class="mb-3">
        <label for="estado" class="form-label">ESTADO</label>
        <div class="input-group">
          <select v-model="estadoSeleccionado" class="form-select" id="ciudades" @change="cargarCiudad">
            <option value="" hidden>Seleccione un estado</option>
            <option v-for="estado in estados" :key="estado.id" :value="estado.id">{{estado.name}}</option>
          </select>
        </div>
  </div>

  <!-- Combobox 3 con v-for-->
  <div class="mb-3">
        <label for="ciudades" class="form-label">CIUDADES</label>
        <div class="input-group">
          <select v-model="ciudadSeleccionada" class="form-select" id="ciudades">
            <option value="" hidden>Seleccione una ciudad</option>
            <option v-for="ciudad in ciudades" :key="ciudad.id" :value="ciudad.id">{{ciudad.name}}</option>
          </select>
        </div>
  </div>

</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data() {
    return {
      paises: [],
      estados: [],
      ciudades: [],
      paisSeleccionado: '',
      estadoSeleccionado: '',
    }
  },
  mounted() {
    this.cargarPaises()
  },
  methods: {
    cargarPaises() {
      axios
        .get('http://localhost:3000/api/v1/country')
        .then((response) => {
          this.paises = response.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
    cargarEstado() {
      console.log(this.paisSeleccionado)
      if (this.paisSeleccionado) {
        axios
          .get(`http://localhost:3000/api/v1/state/country/${this.paisSeleccionado}`)
          .then((response) => {
            this.estados = response.data;
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    cargarCiudad() {
      console.log(this.estadoSeleccionado)
      if (this.estadoSeleccionado) {
        axios
          .get(`http://localhost:3000/api/v1/city/state/${this.estadoSeleccionado}`)
          .then((response) => {
            this.ciudades = response.data;
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },

  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
