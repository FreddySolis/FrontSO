<template lang="html">
  <div class="container">
    <div class="row">
      <div class="col text-left">
        <h2>Listado de Alumnos</h2>
        <b-button class="primary" :to="{name:'NuevoAlumno'}">
          Crear un nuevo estudiante
        </b-button>
        <input placeholder="Buscar" v-model.trim="criteria"></input>
        </div>
        <br>

        <div class="col-md-12">
          <b-table striped hover 
          :items="Alumnos" 
          :fields="fields" 
          :sort-by.sync="sortBy" 
          :sort-desc.sync="sortDesc"
          :filter="criteria"
          responsive="sm"
          >
            <template v-slot:cell(action)="data" >
                <b-button size="sm" variant="primary" :to="{name:'EditarAlumno', params:{estudianteId : data.item.id}}">Editar</b-button>
                <b-button size="sm" variant="danger" v-on:click="Deletetetete(data.item.id)">Eliminar</b-button>
            </template>
          </b-table>
      </div>
    </div>
  </div>
</template>

<script>
import swal from 'sweetalert';
import axios from "axios";
export default {
  data() {
    return {
      criteria: "",
      sortBy: 'nombre',
      sortDesc: false,
      fields: [
        { key: "name", label: "Nombre" , sortable: true },
        { key: "age", label: "Edad" },
        { key: "carrera", label: "Carrera" },
        { key: "action", label: "Acciones" }
      ],
      Alumnos: []
    }
  },
  methods: {

    Deletetetete(x){
      
      swal("Click on either the button or outside the modal.")
  .then((value) => {
    const path="http://backsoquierodormir.ddns.net/api/v1/registro/"+x+""
    console.log("Esto es path "+path)
    axios.delete(path, {headers: { Authorization: localStorage.token}}).then((response) => {
      location.href='/ListAlumno'
    })
  });
    },
    getAlumnos() {
      const path = "http://backsoquierodormir.ddns.net/api/v1/registro/";
      axios
        .get(path, {headers: { Authorization: localStorage.token}})
        .then(response => {
          this.Alumnos = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  created() {
    this.getAlumnos();
  }
};
</script>

<style>
</style>