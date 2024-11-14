<template>
    <div class="login">
      <div class="row justify-content-center">
        <div class="col-md-6">
          <div class="card">
            <div class="card-body">
              <h2 class="card-title text-center mb-4">Connexion</h2>
              <form @submit.prevent="handleLogin">
                <div class="mb-3">
                  <label for="email" class="form-label">Email</label>
                  <input 
                    type="email" 
                    class="form-control" 
                    id="email" 
                    v-model="email" 
                    required
                  >
                </div>
                <div class="mb-3">
                  <label for="password" class="form-label">Mot de passe</label>
                  <input 
                    type="password" 
                    class="form-control" 
                    id="password" 
                    v-model="password" 
                    required
                  >
                </div>
                <button type="submit" class="btn btn-primary w-100">
                  Se connecter
                </button>
              </form>
              <div class="text-center mt-3">
                <router-link to="/register">Pas encore de compte ? S'inscrire</router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'LoginView',
    data() {
      return {
        email: '',
        password: ''
      }
    },
    methods: {
      handleLogin() {
        console.log('Tentative de connexion avec:', {
          email: this.email,
          password: this.password
        })
        // Ici nous ajouterons la logique de connexion plus tard
      }
    }
  }
  </script>