<template>
<section class="src-components-notas">
    <div class="jumbotron">
    <h2>Notas</h2>
    <br>


    <vue-form :state="formstate" @submit.prevent="enviar()">
        
        <validate tag="div">
            <label for="nombre">Nombre</label>
            <input type="text" id="nombre" v-model.trim="formData.nombre" required minlength="3" maxlength="15" no-espacios name="nombre" autocomplete="off" class="form-control"/>
            <field-messages name="nombre" show="$dirty">
                <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
                <div slot="minlength" class="alert alert-danger mt-1">Este campo debe poseer minimo 3 carácteres</div>
                <div v-if="formData.nombre && (formData.nombre.length == 15)" class="alert alert-danger mt-1">Este campo debe poseer máximo 15 carácteres</div>
                <div slot="no-estacios" class="alert alert-danger mt-1">Este campo no permite espacios intermedios</div>
            </field-messages>
        </validate>
        <br>

        <validate tag="div">
            <label for="apellido">Apellido</label>
            <input type="text" id="apellido" v-model.trim="formData.apellido" required minlength="3" maxlength="15" no-espacios name="apellido" autocomplete="off" class="form-control"/>
            <field-messages name="apellido" show="$dirty">
                <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
                <div slot="minlength" class="alert alert-danger mt-1">Este campo debe poseer minimo 3 carácteres</div>
                <div v-if="formData.apellido && (formData.apellido.length == 15)" class="alert alert-danger mt-1">Este campo debe poseer máximo 15 carácteres</div>
                <div slot="no-estacios" class="alert alert-danger mt-1">Este campo no permite espacios</div>
            </field-messages>
        </validate>
        <br>

        <validate tag="div">
            <label for="nota">Nota</label>
            <input type="text" id="nota" v-model.number="formData.nota" required name="nota" autocomplete="off" class="form-control"/>
            <field-messages name="nota" show="$dirty">
                <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
                <div slot="no-estacios" class="alert alert-danger mt-1">Este campo no permite espacios intermedios</div>
            </field-messages>
        </validate>
        <br>
        <button class="btn btn-success my-3" :disabled="formstate.$invalid" type="submit">Enviar</button>
        <br>
    </vue-form>

    <div v-if="alumnos.length" class="tabla-respondive">
    <table class="table">
        <tr>
            <th>Nombre completo</th>
            <th>Nota</th>
        </tr>
        <tr :style="{color: getColor().color}" v-for="(alumno,index) in alumnos" :key="index">
            <td>{{alumno.nombre + " " + alumno.apellido}}</td>
            <td>{{alumno.nota}}</td>
        </tr>
        <tr :style="{color: getPromedio().color}">
        <th>Promedio</th>
        <th>{{getPromedio().promedio}}</th>
        <td></td>
        </tr>

    </table>
    
    </div>
    <h4 v-else class="alert alert-danger">No hay alumnos ingresados</h4>
</div>
</section>
</template>


<script>

    export default {
        name: 'src-components-notas',
        props: [],
        mounted (){

        },

        data(){
            return{
                formstate: {},
                formData: this.getInitialData(),
                alumnos: [],
                promedio: null,
            }
        },

        methods: {
            getInitialData(){
                return{
                    nombre: null,
                    apellido: null,
                    nota: null,
                }
            },

            enviar(){
                let alumno = {...this.formData}

                this.alumnos.push(alumno)

                this.formData = this.getInitialData()
                this.formstate._reset()
            },

            getPromedio(){
                let sumaNotas = 0;
                let contador = 0;
                let promedio = 0;
                this.alumnos.forEach(alumno => {
                    sumaNotas += alumno.nota;
                    contador ++
                })

                promedio = sumaNotas / contador;

                let color = 'red'
                if((promedio >= 4 ) && (promedio < 7)) color = 'yellow'
                else if((promedio >= 7)) color = 'green'

                return {
                    promedio,
                    color
                }
               
            },
            
            getColor(){
               
               let color = 'red'
                    
                    if((this.getInitialData().nota > 4) && (this.getInitialData().nota < 7)) color = 'yellow'
                    else if((this.getInitialData().nota > 7)) color = 'green'
            
                return {
                    color
                }
            }
        },

        computed: {

        }
    }
</script>

<style scoped lang="css">
  .src-componentes-ingreso {

  }

  .jumbotron {
    background: #fff;
    color: #333;
  }

  hr {
    background-color: #eee;
  }

  pre {
    color: white;
  }
</style>