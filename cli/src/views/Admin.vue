<template>
    <div class="tableadmin">        
        <b-alert
        :show="dismissCountDown"
        dismissible
        :variant="mensaje.color"
        @dismissed="dismissCountDown=0"
        @dismiss-count-down="countDownChanged">
        {{mensaje.texto}}
        </b-alert>
        <br>
        <h2>ADMINISTRACIÓN DE TABLAS MONGODB</h2>        
        <b-card no-body>
            <b-tabs pills card>
            <!-- Pestaña Estudiantes-->
            <b-tab title="Estudiantes" active>
            <br>
            <h2>Tabla Estudiantes</h2>
            <hr>
            <table table class="table table-hover table-striped table-responsive">
            <thead>
            <tr tr class="table-dark">
            <th scope="col"># MongoDB</th>
            <th scope="col">No. Identificación</th>
            <th scope="col">Contraseña</th>
            <th scope="col">Nombre(s)</th>
            <th scope="col">Apellido(s)</th>
            <th scope="col">Correo</th>
            <th scope="col">Fecha de Nacimiento</th>
            <th scope="col">Colegio</th>
            <th scope="col">Ciudad</th>
            <th scope="col">Acciones</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(item, index) in estudiantes" :key="index">
            <th scope="row">{{item._id}}</th>
            <td>{{item.numident}}</td>
            <td>{{item.password}}</td>
            <td>{{item.nombre}}</td>
            <td>{{item.apellido}}</td>
            <td>{{item.correo}}</td>
            <td>{{item.fechanac}}</td>
            <td>{{item.colegio}}</td>
            <td>{{item.ciudad}}</td>
            <td>
	        <b-button @click="activarEdicion(item.numident)" class="btn-warning btn-sm">Editar</b-button>
		<b-button @click="eliminarEstud(item._id)" class="btn-danger btn-sm mx-2">Eliminar</b-button>
                
            </td>
            </tr>
            </tbody>
            </table>
            <!-- Formulario de Editar-->
            <form @submit.prevent="editarEstudiante(estudianteEditar)" v-if="editar">
            <h2>Modificar Estudiante</h2>
            <input type="text" class="form-control my-2" placeholder="Número de Identificación Personal" v-model="estudianteEditar.numident">
            <input type="text" class="form-control my-2" placeholder="Contraseña" v-model="estudianteEditar.password">
            <input type="text" class="form-control my-2" placeholder="Nombre(s)" v-model="estudianteEditar.nombre">
            <input type="text" class="form-control my-2" placeholder="Apellido(s)" v-model="estudianteEditar.apellido">
            <input type="text" class="form-control my-2" placeholder="Correo Electrónico" v-model="estudianteEditar.correo">
            <input type="text" class="form-control my-2" placeholder="Fecha de Nacimiento (dd/mm/aaaa)" v-model="estudianteEditar.fechanac">
            <input type="text" class="form-control my-2" placeholder="Colegio" v-model="estudianteEditar.colegio">
            <input type="text" class="form-control my-2" placeholder="Ciudad" v-model="estudianteEditar.ciudad">
            <b-button class="btn-warning my-2 mx-2" type="submit">Guardar</b-button>
            <b-button class="my-2" type="submit" @click="editar = false">Cancelar</b-button>
            </form>
            <!-- Formulario de Agregar-->
            <form class="agregarest" @submit.prevent="agregarEstud(objEstudiante)" v-if="!editar">
            <br>
            <hr>
            <h2>Agregar Estudiante</h2>
            <input type="text" class="form-control my-2" placeholder="Número de Identificación Personal" v-model="objEstudiante.numident">
            <input type="password" class="form-control my-2" placeholder="Contraseña" v-model="objEstudiante.password">
            <input type="text" class="form-control my-2" placeholder="Nombre(s)" v-model="objEstudiante.nombre">
            <input type="text" class="form-control my-2" placeholder="Apellido(s)" v-model="objEstudiante.apellido">
            <input type="email" class="form-control my-2" placeholder="Correo Electrónico" v-model="objEstudiante.correo">
            <input type="date" class="form-control my-2" placeholder="Fecha de Nacimiento (dd/mm/aaaa)" v-model="objEstudiante.fechanac">
            <input type="text" class="form-control my-2" placeholder="Colegio" v-model="objEstudiante.colegio">
            <input type="text" class="form-control my-2" placeholder="Ciudad" v-model="objEstudiante.ciudad">
            <b-button class="btn-success my-2" type="submit">Agregar</b-button>
            </form>
            </b-tab>
            <!-- Pestaña Resultados-->
            <b-tab title="Resultados">
            <br>
            <h2>Tabla Resultados</h2>
            <hr>
            <table class="table table-hover table-striped table-responsive">
            <thead>
                <tr tr class="table-dark">
                <th scope="col">Mongo ID</th>
                <th scope="col">ID Estudiante</th>
                <th scope="col">Estado</th>
                <th scope="col">Resultado Intereses Letra</th>
                <th scope="col">Resultado Intereses Puntaje</th>
                <th scope="col">Rama Sugerida Intereses</th>                        
                <th scope="col">Resultado Aptitudes Letra</th>                
                <th scope="col">Resultado Aptitudes Puntaje</th>
                <th scope="col">Rama Sugerida Aptitudes</th>
                <th scope="col">Acciones</th>
                </tr>
            </thead>
            <tbody> 
                <tr v-for="(item, index) in resultados" :key="index" class="table-secondary"> 
                <th scope="row">{{item._id}}</th>
                <td>{{item.numident}}</td>
                <td>{{item.estado}}</td>
                <td>{{item.letraintereses}}</td>
                <td>{{item.puntajeintereses}}</td>
                <td>{{item.ramasugerint}}</td>
                <td>{{item.letraaptitudes}}</td>
                <td>{{item.puntajeaptitudes}}</td>
                <td>{{item.ramasugerapt}}</td>
                <td><b-button @click="eliminarResultado(item._id)" class="btn-danger btn-sm mx-2">Eliminar</b-button></td>
                </tr>     
            </tbody>
            </table>
            </b-tab>
            <!-- Pestaña Contáctenos-->
            <b-tab title="Contactos">
            <br>
            <h2>Tabla Contáctenos</h2>
            <hr>
            <table class="table table-hover table-striped table-responsive">
            <thead>
                <tr tr class="table-dark">
                <th scope="col"># MongoDB</th>
                <th scope="col">Nombre y Apellido</th>
                <th scope="col">Correo Electrónico</th>
                <th scope="col">Institución</th>
                <th scope="col">Mensaje</th>
                <th scope="col">Acciones</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) in contactos" :key="index" class="table-secondary">
                <th scope="row">{{item._id}}</th>
                <td>{{item.nombreapellido}}</td>
                <td>{{item.correo}}</td>
                <td>{{item.institucion}}</td>
                <td>{{item.mensaje}}</td>
                <td><b-button @click="eliminarContacto(item._id)" class="btn-danger btn-sm mx-2">Eliminar</b-button></td>
                </tr>     
            </tbody>
            </table>   
            </b-tab>
        </b-tabs>
        </b-card>
</div>
</template>
<script>
export default {
    data() {
        return {
            listado:[],
            estudiantes:[],
            contactos:[],
            resultados:[],
            objEstudiante:{numident:'', password:'', nombre:'', apellido:'', correo:'', fechanac:'', colegio:'', ciudad:''},
            editar: false,
            estudianteEditar: {},
            mensaje: {color: '', texto: ''},
            dismissSecs: 5,
            dismissCountDown: 0
            
        }
    },
    created() {
        this.listarEstudiantes();
        this.listarResultados();
        this.listarContactos();

    },    
    
    methods: {
        countDownChanged(dismissCountDown) {
            this.dismissCountDown = dismissCountDown
        },
        
        showAlert() {
            this.dismissCountDown = this.dismissSecs
        },

        alerta(){
            this.mensaje.color = 'danger';
            this.mensaje.texto = 'Probando alerta';
            this.showAlert();
        },        

        listarEstudiantes(){
            this.axios.get('/estudiante')
            .then(res=>{
                console.log(res.data)
                this.estudiantes = res.data
            })
            .catch(e=>{
                console.log(e.response)
            })
        },
        
        listarResultados(){
            this.axios.get('/resultado')
            .then(res=>{
                console.log(res.data)
                this.resultados = res.data
            })
            .catch(e=>{
                console.log(e.response)
            })
        },

        listarContactos(){
        this.axios.get('/contacto')
            .then(res=>{
                console.log(res.data)
                this.contactos = res.data
            })
            .catch(e=>{
                console.log(e.response)
            })
        },

        agregarEstud(){
            this.axios.post('/nuevo-estudiante', this.objEstudiante)
            .then(res=>{
                this.estudiantes.push(res.data)
                this.objEstudiante.numident='';
                this.objEstudiante.password='';
                this.objEstudiante.nombre='';                
                this.objEstudiante.apellido='';
                this.objEstudiante.correo='';
                this.objEstudiante.fechanac='';
		        this.objEstudiante.colegio='';
                this.objEstudiante.ciudad='';
                //Limpieza de los campos
                this.mensaje.color='Success';
                this.mensaje.texto='¡Estudiante Agregado!'
                this.showAlert()
            })
            .catch(e=>{
                console.log(e.response);
                if(e.response.data.error.errors.nombre.message){
                    this.mensaje.texto = e.response.data.error.errors.nombre.message
                } else{
                    this.mensaje.texto = '¡Error del Sistema!';
                }
                this.mensaje.color = 'danger';
                this.showAlert()
            })
        },

        eliminarEstud(id){
            console.log(id);
            this.axios.delete(`/estudiante/${id}`)
            .then(res => {
                const index = this.estudiantes.findIndex(item => item._id === res.data._id);
                this.estudiantes.splice(index, 1);
                this.mensaje.color = 'success';
                this.mensaje.texto = '¡Estudiante Borrado!';
                this.showAlert();
            })
            .catch(e => {
                console.log(e.response);
            })
        },
        
        eliminarResultado(id){
            this.axios.delete(`/resultado/${id}`)
            .then(res => {
                const index = this.resultados.findIndex(item => item._id === res.data._id);
                this.resultados.splice(index, 1);
                this.mensaje.color = 'success';
                this.mensaje.texto = '¡Resultado Borrado!';
                this.showAlert();
            })
            .catch(e => {
                console.log(e.response);
            })
        },

        eliminarContacto(id){
            console.log(id);
            this.axios.delete(`/contacto/${id}`)
            .then(res => {
                const index = this.contactos.findIndex(item => item._id === res.data._id);
                this.contactos.splice(index, 1);
                this.mensaje.color = 'success';
                this.mensaje.texto = '¡Contacto Borrado!';
                this.showAlert();
            })
            .catch(e => {
                console.log(e.response);
            })
        },

        activarEdicion(numident){
            this.editar = true;
            this.axios.get(`/estudiante/${numident}`)
            .then(res => {
                this.estudianteEditar = res.data;
            })
            .catch(e => {
               console.log(e.response);
            })
        },
        
        editarEstudiante(item){
            this.axios.put(`/estudiante/${item._id}`, item)
            .then(res => {
            const index = this.estudiantes.findIndex(n => n._id === res.data._id);
            this.estudiantes[index].numident = res.data.numident;
            this.estudiantes[index].password = res.data.password;
            this.estudiantes[index].nombre = res.data.nombre;
            this.estudiantes[index].apellido = res.data.apellido;
            this.estudiantes[index].correo = res.data.correo;
            this.estudiantes[index].fechanac = res.data.fechanac;
	        this.estudiantes[index].colegio = res.data.colegio;
            this.estudiantes[index].ciudad = res.data.ciudad;
            this.mensaje.color = 'success';
            this.mensaje.texto = '¡Estudiante Actualizado!';
            this.showAlert();
            this.editar = false;
            })
            .catch(e => {
                console.log(e.response);
            })
        }
    },
  }
</script>
<style lang="scss">
.agregarest{
    width: 700px;
    
    align-content: center;
    align-items: center;
}
.table{
    table-layout: fixed;
}
.tableadmin{
    font-family: Verdana, sans-serif;
    overflow: auto;
    height: 527px;
    align-items: center;
}
footer {
  font-family: Verdana, sans-serif;
  color: #fff;
  min-height: 190px;
  width: 100%;
  font-size: 80%;
  background: #0d6efd;
  }

h2 {
  font-family: "Inter", sans-serif;
	color: mix(#fff, #152beb, 10%);
	font-size: calc(0.5em + 1.2vw);
	font-weight: 800;
	text-shadow: 
		-0.0075em 0.0075em 0 mix(#fff, #b3c9f1, 94%),
		0.005em 0.005em 0 mix(#fff, #b3c9f1, 60%),
		0.01em 0.01em 0 mix(#fff, #b3c9f1, 62%), 
		0.015em 0.015em mix(#fff, #b3c9f1, 64%), 
		0.02em 0.02em 0 mix(#fff, #b3c9f1, 66%), 
		0.025em 0.025em 0 mix(#fff, #b3c9f1, 68%),
		0.03em 0.03em 0 mix(#fff, #b3c9f1, 70%),
		0.035em 0.035em 0 mix(#fff, #b3c9f1, 72%);
}
</style>