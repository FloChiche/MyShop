<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container">
      <router-link class="navbar-brand" to="/">MyShop</router-link>
      <div class="navbar-nav">
        <router-link class="nav-item nav-link" to="/">Accueil</router-link>
        <router-link class="nav-item nav-link" to="/login">Connexion</router-link>
        <router-link class="nav-item nav-link" to="/register">Inscription</router-link>
        <router-link class="nav-item nav-link" to="/admin">Admin</router-link>
      </div>
      <div class="ms-auto">
        <button 
          class="btn btn-link position-relative"
          @click="showCart = !showCart"
        >
          <i class="bi bi-cart fs-4 text-primary"></i>
          <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">
            {{ cartStore.totalItems }}
          </span>
        </button>
      </div>
    </div>
  </nav>

  <main class="container mt-4">
    <router-view></router-view>
  </main>

  <!-- Ajout du composant CartSidebar -->
  <CartSidebar 
    v-model:isOpen="showCart"
  />
</template>

<script>
import { useCartStore } from './stores/cartStore'
import CartSidebar from './components/CartSidebar.vue'
import { ref } from 'vue'

export default {
  name: 'App',
  components: {
    CartSidebar
  },
  setup() {
    const cartStore = useCartStore()
    const showCart = ref(false)

    return { 
      cartStore,
      showCart
    }
  }
}
</script>

<style>
.navbar {
  margin-bottom: 2rem;
}

.btn-link {
  text-decoration: none;
  padding: 0;
}

.badge {
  font-size: 0.6rem;
  padding: 0.35em 0.65em;
}
</style>