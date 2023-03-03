<template>
    <div>
        <input v-model="user" placeholder="Intruzca su nombre de usuario de GitHub" v-show="input" v-on:keydown="obtenerUsuario"/>
        <div class="alert alert-danger" role="alert" v-show="advertencia">
            El usuario no existe
        </div>
        <div class="card">
            <img :src="user.avatar_url" class="card-img-top" alt="">
            <div class="card-body">
                <h6 class="card-title text-center">{{ user.login }}</h6>
                <button class="btn btn-primary mx-auto" @click = "obtenerRepositorios">Repositorios</button>
                <a :href="'https://github.com' + user.login" class="btn mx-auto"> URL de GitHub</a>
            </div>
        </div>
        <div v-show="mostrarRepo" class="row">
            <div v-for="r in repos" :key="r.id" class="card col-5">
                <div class="card-body">
                    <h6 class="card-title">{{ r.name }}</h6>
                    <p class="card-text">{{ r.description }}</p>
                    <a :href="r.html_url" class="btn btn-primary mx-auto">Ver Repositorio</a>
                </div>
            </div>
        </div>
        <GitHubRepos></GitHubRepos>
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
            input: true,
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
            var userAuth = process.env.VUE_APP_USERNAME || "AuroraAF";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            // TODO: realizar petición fetch par obtener los datos y mostrar la información en la página
            // Ejemplo de paso de datos de autorización con fetch: https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch
            let url = 'https://api.github.com/users/{{ user }}';
            fetch(url, {method: 'GET'})
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
            var userAuth = process.env.VUE_APP_USERNAME || "AuroraAF";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";


            // TODO: realizar petición fetch par obtener los datos y mostrar la información en la página
            // Ejemplo de paso de datos de autorización con fetch: https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch
            let url = 'https://api.github.com/users/{{ user }}/repos';
            fetch(url, {method: 'GET'})
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
