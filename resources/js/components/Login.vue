<template>
    <!--    <div class="container h-100">
            <div class="row h-100 align-items-center">
                <div class="col-12 col-md-6 offset-md-3">
                    <div class="card shadow sm">
                        <div class="card-body">
                            <h1 class="text-center">Login</h1>
                            <hr/>
                            <form action="javascript:void(0)" class="row" method="post">
                                <div class="col-12" v-if="Object.keys(validationErrors).length > 0">
                                    <div class="alert alert-danger">
                                        <ul class="mb-0">
                                            <li v-for="(value, key) in validationErrors" :key="key">{{ value[0] }}</li>
                                        </ul>
                                    </div>
                                </div>
                                <div class="form-group col-12">
                                    <label for="email" class="font-weight-bold">Email</label>
                                    <input type="text" v-model="auth.email" name="email" id="email" class="form-control">
                                </div>
                                <div class="form-group col-12 my-2">
                                    <label for="password" class="font-weight-bold">Password</label>
                                    <input type="password" v-model="auth.password" name="password" id="password" class="form-control">
                                </div>
                                <div class="col-12 mb-2">
                                    <button type="submit" :disabled="processing" @click="login" class="btn btn-primary btn-block">
                                        {{ processing ? "Please wait" : "Login" }}
                                    </button>
                                </div>
                                <div class="col-12 text-center">
                                    <label>Don't have an account? <router-link :to="{name:'register'}">Register Now!</router-link></label>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>-->
    <!-- Barra de navegación -->

    <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top">
        <a class="navbar-brand" href="#">Mi Sitio</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarResponsive" aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarResponsive">
            <ul class="navbar-nav ml-auto">
                <li class="nav-item active">
                    <a class="nav-link" href="#">Inicio
                        <span class="sr-only">(current)</span>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Acerca de</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Servicios</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Contacto</a>
                </li>
            </ul>
        </div>
    </nav>

    <!-- Contenido principal -->
    <main>

        <div class="contenedor__todo">
            <div class="caja__trasera">
                <div class="caja__trasera-login">
                    <h3>¿Ya tienes una cuenta?</h3>
                    <p>Inicia sesión para entrar en la página</p>
                    <button id="btn__iniciar-sesion" @click="iniciarSesion()">Iniciar Sesión</button>
                </div>
                <div class="caja__trasera-register">
                    <h3>¿Aún no tienes una cuenta?</h3>
                    <p>Regístrate para que puedas iniciar sesión</p>
                    <button id="btn__registrarse" @click="register()">Regístrarse</button>
                </div>
            </div>

            <!--Formulario de Login y registro-->
            <div class="contenedor__login-register">
                <!--Login-->
                <form action="javascript:void(0)" method="POST" class="formulario__login">
                    <h2>Iniciar Sesión</h2>
                    <input type="text" placeholder="Correo Electronico" v-model="auth.email" name="email" id="email">
                    <input type="password" placeholder="Contraseña" v-model="auth.password" name="password" id="password">
                    <button @click="login">Entrar</button>
                </form>

                <!--Register-->
                <form action="javascript:void(0)" method="POST" class="formulario__register">
                    <h2>Regístrarse</h2>
                    <input type="text" placeholder="Nombre completo" v-model="user.family_name" name="nombre_completo">
                    <input type="text" placeholder="Correo" v-model="user.email" name="correo">
                    <input type="text" placeholder="Usuario" v-model="user.name" name="usuario">
                    <input type="password" placeholder="Contrasena" v-model="user.password" name="contrasena">
                    <button @click="registerForm()">Regístrarse</button>
                </form>
            </div>
        </div>
        <div class="col-12" v-if="Object.keys(validationErrors).length > 0">
            <div class="alert alert-danger">
                <ul class="mb-0">
                    <li v-for="(value, key) in validationErrors" :key="key">{{ value[0] }}</li>
                </ul>
            </div>
        </div>

    </main>

    <!--tabla de bd  =  login_register_db-->

    <!-- Pie de página -->
    <footer>
        Desarrollado por Michael Perez |
        <a href="https://www.markingwebs.com" target="_blank">MarkingWebs</a> |
        <a href="https://www.linkedin.com/in/michael-alexander-perez-1643b2236/" target="_blank">LinkedIn</a>
    </footer>
</template>

<script>
import { mapActions } from 'vuex'

export default {
    name:"login",
    data(){
        return {
            auth:{
                email:"",
                password:""
            },
            user:{
                name:"",
                family_name:"",
                email:"",
                password:"",
                password_confirmation:""
            },
            validationErrors:{},
            processing:false
        }
    },
    mounted() {
        this.anchoPage();
    },
    methods:{
        ...mapActions({
            signIn:'auth/login'
        }),
        async login(){
            this.processing = true
            await axios.get('/sanctum/csrf-cookie')
            await axios.post('/login',this.auth).then(({data})=>{
                this.signIn()
            }).catch(({response})=>{
                if(response.status===422){
                    this.validationErrors = response.data.errors
                }else{
                    this.validationErrors = {}
                    alert(response.data.message)
                }
            }).finally(()=>{
                this.processing = false
            })
        },
        async registerForm(){
            this.processing = true
            this.user.password_confirmation = this.user.password
            await axios.get('/sanctum/csrf-cookie')
            await axios.post('/register',this.user).then(response=>{
                this.validationErrors = {}
                this.signIn()
            }).catch(({response})=>{
                if(response.status===422){
                    this.validationErrors = response.data.errors
                }else{
                    this.validationErrors = {}
                    alert(response.data.message)
                }
            }).finally(()=>{
                this.processing = false
            })
        },
        anchoPage(){
            var formulario_login = document.querySelector(".formulario__login");
            var formulario_register = document.querySelector(".formulario__register");
            var contenedor_login_register = document.querySelector(".contenedor__login-register");
            var caja_trasera_login = document.querySelector(".caja__trasera-login");
            var caja_trasera_register = document.querySelector(".caja__trasera-register");
            if (window.innerWidth > 850){
                caja_trasera_register.style.display = "block";
                caja_trasera_login.style.display = "block";
            }else{
                caja_trasera_register.style.display = "block";
                caja_trasera_register.style.opacity = "1";
                caja_trasera_login.style.display = "none";
                formulario_login.style.display = "block";
                contenedor_login_register.style.left = "0px";
                formulario_register.style.display = "none";
            }
        },
        iniciarSesion(){
            var formulario_login = document.querySelector(".formulario__login");
            var formulario_register = document.querySelector(".formulario__register");
            var contenedor_login_register = document.querySelector(".contenedor__login-register");
            var caja_trasera_login = document.querySelector(".caja__trasera-login");
            var caja_trasera_register = document.querySelector(".caja__trasera-register");
            if (window.innerWidth > 850){
                formulario_login.style.display = "block";
                contenedor_login_register.style.left = "10px";
                formulario_register.style.display = "none";
                caja_trasera_register.style.opacity = "1";
                caja_trasera_login.style.opacity = "0";
            }else{
                formulario_login.style.display = "block";
                contenedor_login_register.style.left = "0px";
                formulario_register.style.display = "none";
                caja_trasera_register.style.display = "block";
                caja_trasera_login.style.display = "none";
            }
        },
        register(){
            var formulario_login = document.querySelector(".formulario__login");
            var formulario_register = document.querySelector(".formulario__register");
            var contenedor_login_register = document.querySelector(".contenedor__login-register");
            var caja_trasera_login = document.querySelector(".caja__trasera-login");
            var caja_trasera_register = document.querySelector(".caja__trasera-register");
            if (window.innerWidth > 850){
                formulario_register.style.display = "block";
                contenedor_login_register.style.left = "410px";
                formulario_login.style.display = "none";
                caja_trasera_register.style.opacity = "0";
                caja_trasera_login.style.opacity = "1";
            }else{
                formulario_register.style.display = "block";
                contenedor_login_register.style.left = "0px";
                formulario_login.style.display = "none";
                caja_trasera_register.style.display = "none";
                caja_trasera_login.style.display = "block";
                caja_trasera_login.style.opacity = "1";
            }
        }
    }
}


</script>
