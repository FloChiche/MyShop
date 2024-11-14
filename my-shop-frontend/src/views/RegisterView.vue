<template>
  <div class="register">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <h2 class="card-title text-center mb-4">Inscription</h2>
            <form @submit.prevent="handleRegister">
              <div class="mb-3">
                <label for="username" class="form-label">Nom d'utilisateur</label>
                <input 
                  type="text" 
                  class="form-control" 
                  id="username" 
                  v-model="username" 
                  required
                >
              </div>
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
              <div class="mb-3">
                <label for="confirmPassword" class="form-label">Confirmer le mot de passe</label>
                <input 
                  type="password" 
                  class="form-control" 
                  id="confirmPassword" 
                  v-model="confirmPassword" 
                  required
                >
              </div>
              <button type="submit" class="btn btn-primary w-100">
                S'inscrire
              </button>
            </form>
            <div class="text-center mt-3">
              <router-link to="/login">Déjà un compte ? Se connecter</router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RegisterView',
  data() {
    return {
      username: '',
      email: '',
      password: '',
      confirmPassword: ''
    }
  },
  methods: {
    handleRegister() {
      if (this.password !== this.confirmPassword) {
        alert('Les mots de passe ne correspondent pas')
        return
      }
      console.log('Tentative d\'inscription avec:', {
  username: this.username,
  email: this.email,
  password: this.password
})
      // Ici nous ajouterons la logique d'inscription plus tard
    }
  }
}
</script>