<template>
    <div class="container-fluid">
     <input type="text" id="nombre" class="form-control" @keyup.enter = "obtenerUsuario">
 
     <div class="alert alert-danger" role="alert" v-if="advertencia">
       El usuario no existe
     </div>
 
     <div v-else>
       <div class="card mx-auto" v-if="card">
         <img :src="user.avatar_url" class="card-img-top" alt="...">
         <div class="card-body">
           <h5 class="card-title text-center">{{ user.login }}</h5>
           <button class="btn btn-primary mx-auto" @click = "obtenerRepositorios">Repositorios</button>
           <a :href="'https://github.com/' + user.login" target="_blank" class="btn btn-primary mx-auto mt-5">Url GitHub</a>
         </div>
       </div>
 
       <div v-if="mostrarRepo"  class="row">
         <div v-for="repo in repos" :key="repo.id" class="card col-4">
       
           <div class="card-body">
             <h5 class="card-title">{{ repo.name }}</h5>
             <p class="card-text">{{ repo.description }}</p>
             <a :href="repo.html_url" target="_blank" class="btn btn-primary mt-2">Ver repositorio</a>
           </div>
         </div>
       </div>
     </div>
    <GitHubRepos />
   </div>
 </template>

<script>

// TODO: Importar componente GitHubRepos
import GitHubRepos from './GitHubRepos.vue'

export default {
    name: 'GitHub',
    components: {
        // TODO: Importar componente GitHubRepos
        GitHubRepos
    },
    data: function() {
        return {
            user: null,
            advertencia: false,
            mostrarRepo: false,
            card: false,
            repos: [],
        }
    },
    methods: {
        obtenerUsuario: function() {
            // TODO: Función para obtener los datos de usuario de la API de GitHub

            // TODO: Añadir lógica para resetear los cambios en el interfaz: desactivar campo de envío,
            // resetear mensaje de error, mostrar lista de repositorios,...

            // Obtener datos de autenticación de usuario para hacer peticiones
            // autenticadas a la API de GitHub
            //var userAuth = process.env.VUE_APP_USERNAME || "AuroraAF";
            //var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            // TODO: realizar petición fetch par obtener los datos y mostrar la información en la página
            // Ejemplo de paso de datos de autorización con fetch: https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch
            fetch(`https://api.github.com/users/${document.getElementById("nombre").value}`)
            .then((response => {
                if(response.ok) {
                    return response.json();
                } 
                else {
                    throw new Error('Error');
                }
            }))
            .then((data) => {
                this.user = data;
                this.advertencia = false;
                this.card = true;
                this.mostrarRepo = false;
                this.repos = [];
            })
            .catch(() => {
                this.advertencia = true;
                this.card = false;
                this.mostrarRepo = false;
            });
        },
        obtenerRepositorios: function() {
            // TODO: Función para obtener los repositorios del usuario desde la API de GítHub
            // La URL de los repositorios de usuario se puede obtener a través del campo 'repos_url' de los datos del usuario

            // Obtener datos de autenticación de usuario para hacer peticiones
            // autenticadas a la API de GitHub
            //var userAuth = process.env.VUE_APP_USERNAME || "AuroraAF";
            //var passAuth = process.env.VUE_APP_USERTOKEN || "pass";


            // TODO: realizar petición fetch par obtener los datos y mostrar la información en la página
            // Ejemplo de paso de datos de autorización con fetch: https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch
            
            fetch(`https://api.github.com/users/${this.user.login}/repos`,)
            .then((response => {
                if(response.ok) {
                    return response.json();
                } 
                else {
                    throw new Error('Error');
                }
            }))
            .then((data) => {
                this.repos = data;
                this.mostrarRepo = true;
                this.card = false;
            })
            .catch(error => console.log(error));
        },
    },
};
</script>

<style scoped>
</style>
