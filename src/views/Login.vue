<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12 col-sm-10 col-md-8 offset-sm-1 offset-md-2">
        <div class="mt-5">
          <form class="border border-primary rounded form-inline" @submit="login">

            <h2 class="col-12 text-center text-primary mt-3 mb-5">Iniciar Sesi&oacute;n</h2>

            <div class="form-group col-12">
              <label for="username" class="custom-label col-md-3">Nombre de Usuario</label>
              <input id="username" class="form-control col-12 col-sm-10 col-md-7 offset-sm-1" type="text"
                     placeholder="Nombres" v-model="username" required/>
            </div>

            <div class="form-group col-12">
              <label for="password" class="custom-label col-md-3">Contrase&ntilde;a</label>
              <input id="password" class="form-control col-12 col-sm-10 col-md-7 offset-sm-1" type="password"
                     placeholder="Contraseña" v-model="password" required/>
            </div>

            <div class="col-12 col-sm-6 col-md-5 offset-md-2 text-center mb-3">
                <span class="text-primary">
                  <small>
                    <router-link :to="{ name: 'signup' }">&iquest;No tienes una cuenta? &iexcl;Registrate!</router-link>
                  </small>
                </span>
            </div>
            <div class="col-12 col-sm-5 col-md-4 mb-3">
              <button class="col-sm-10 col-md-10 offset-sm-1 offset-md-2 btn btn-primary" type="submit">
                Iniciar Sesi&oacute;n
              </button>
            </div>

          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
    import axios from 'axios';
    import {setAuthenticationToken} from '@/dataStorage';

    const path = "/oauth/token";

    export default {
        name: "Login.vue",
        components: {},
        data(){
            return {
                username: '',
                password: ''
            }
        },
        methods: {
            login( event ){
                axios
                .post( this.$store.state.backURL + path, // URL
                    { }, // Body
                    {
                        headers: {
                          'Content-Type': 'application/json'
                        },
                        params: {
                            username: this.username,
                            password: this.password,
                            grant_type: 'password'
                        },
                        auth: {
                            username: "soft-eng-ii",
                            password: "secret",
                        }
                    }
                ).then( response => {
                    if( response.status !== 200 ){
                        alert( "Error en la autenticación" );
                    }else{
                        setAuthenticationToken( response.data.access_token );
                        this.$router.push( '../principal' );
                    }
                } ).catch( error => {
                    if( error.response.status === 400 ){
                      alert( "Credenciales incorrectas" );
                    }else{
                      alert( "¡Parece que hubo un error de comunicación con el servidor!" );
                    }
                } );

                event.preventDefault();
            }
        }
    }
</script>

<style scoped>
  form {
    border-radius: 20px !important;
  }

  .form-inline {
    width: auto;
  }

  .form-inline .form-group {
    margin-bottom: 1rem;
  }

  .custom-label {
    display: inline-block;
    justify-content: right !important;
    text-align: right !important;
  }

  @media (max-width: 767px) {
    .custom-label {
      display: none !important;
    }
  }
</style>
