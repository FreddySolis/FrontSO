<template >
<div class="container">
    <div class="row">
        <div class="col text-left">
            <h2>
                Editar Alumno
            </h2>
        </div>
    </div>

    <div class="row">
        <div class="col">
            <div class="card">
                <div class="card-body">
                    <form @submit="onSubmit">
                        <div class="form-group row">
                            <label for="name" class="col-sm-2 col-form-label">name</label>
                            <div class="col-sm-6">
                                <input type="text" placeholder="name" name="name" class="form-control" v-model.trim="form.name"> 
                            </div>
                        </div>
                        <div class="form-group row">
                            <label for="lastname" class="col-sm-2 col-form-label">lastname</label>
                            <div class="col-sm-6">
                                <input type="text" placeholder="lastname" name="lastname" class="form-control" v-model.trim="form.lastname">
                            </div>
                        </div>
                        <div class="form-group row">
                            <label for="age" class="col-sm-2 col-form-label">age</label>
                            <div class="col-sm-6">
                                <input type="text" placeholder="age" name="age" class="form-control" v-model.trim="form.age">
                            </div>
                        </div>
                        <div class="form-group row">
                            <label for="gender" class="col-sm-2 col-form-label">gender</label>
                            <div class="col-sm-6">
                                <input type="text" placeholder="gender" name="gender" class="form-control" v-model.trim="form.gender">
                            </div>
                        </div>
                        <div class="form-group row">
                            <label for="address" class="col-sm-2 col-form-label">Dirección</label>
                            <div class="col-sm-6">
                                <input type="text" placeholder="Dirección" name="address" class="form-control" v-model.trim="form.address">
                            </div>
                        </div>
                        <div class="form-group row">
                            <label for="carrera" class="col-sm-2 col-form-label">Carrera</label>
                            <div class="col-sm-6">
                                <select v-model.trim="form.carrera" class="form-control">
                                <option disabled value="">Seleccione una opcion</option>
                                <option> IDS</option>
                                <option> Meca</option>
                                <option> Bio</option>
                                <option> Ambiental</option>
                                <option> Agro</option>
                                <option> petrolera</option>
                                <option> Manofacturas</option>
                            </select>
                            </div>
                        </div>


                        <div class="rows">
                            <div class="col text-left">
                                <b-button type="submit" variant="primary">Editar</b-button>
                                <b-button type="submit" class="btn-large-space" :to="{name:'ListAlumno'}" variant="danger">Cancelar</b-button>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
  
</template>

<script>
import swal from 'sweetalert';
import axios from 'axios'
export default {
    
    data () {
        return{
            estudianteId: this.$route.params.estudianteId,
            form:{
                name:'',
                lastname:'',
                age:'',
                gender:'',
                address:'',
                carrera:''
            }
        }
    },
    methods:{
        onSubmit(evt){
            const path = `http://ec2-54-197-214-216.compute-1.amazonaws.com/api/v1/r registro/${this.estudianteId}`
            axios.put(path, this.form, {headers: { Authorization: localStorage.token}}).then((response) => {
                this.form.name = response.data.name
                this.form.lastname = response.data.lastname
                this.form.age = response.data.age
                this.form.gender = response.data.gender
                this.form.carrera = response.data.carrera
                this.form.address = response.data.address
                swal ( "Se ha editado correctamente!" ,  "Todo bien" ,  "success" ).then(response => {
                    location.href='/ListAlumno'
                })
            }).catch((error) => {
                swal ( "Not good" ,  "Something went bad!" ,  "error" )
            })
            evt.preventDefault()
        },
        getAlumno(){
            const path = `http://ec2-54-197-214-216.compute-1.amazonaws.com/api/v1/r registro/${this.estudianteId}`
            axios.get(path, {headers: { Authorization: localStorage.token}}).then((response) => {
                this.form.name = response.data.name
                this.form.lastname = response.data.lastname
                this.form.age = response.data.age
                this.form.gender = response.data.gender
                this.form.carrera = response.data.carrera
                this.form.address = response.data.address
                
            }).catch((error) => {
                swal ( "Not good" ,  "Something went bad!" ,  "error" )
            })
        }
    }, 
    created(){
       this.getAlumno()
    }

}
</script>

<style>

</style>