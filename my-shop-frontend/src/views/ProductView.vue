<template>
    <div class="product-page p-4">
      <!-- Notification -->
      <div 
        class="notification" 
        :class="{ 'show': showNotification }"
      >
        Produit ajouté au panier !
        <button class="btn btn-primary btn-sm ms-3" @click="showNotification = false">OK</button>
      </div>
  
      <!-- Fil d'Ariane -->
      <div class="mb-4">
        <router-link to="/" class="text-primary text-decoration-none">Accueil</router-link>
        <span class="mx-2">/</span>
        <router-link to="/" class="text-primary text-decoration-none">Catégorie</router-link>
      </div>
  
      <div class="row">
        <!-- Image du produit -->
        <div class="col-md-6">
          <img 
            :src="currentProduct.image"
            class="img-fluid rounded"
            :alt="currentProduct.name"
          >
        </div>
  
        <!-- Informations produit -->
        <div class="col-md-6">
          <h1 class="h2 mb-4">{{ currentProduct.name }}</h1>
          <p class="text-primary h2 mb-4">{{ currentProduct.price }} €</p>
          <p class="text-muted mb-4">{{ currentProduct.description }}</p>
  
          <!-- Sélecteur de quantité -->
          <div class="mb-4">
            <label class="mb-2">Quantité</label>
            <div class="d-flex align-items-center" style="width: 120px;">
              <button 
                class="btn btn-outline-primary px-3"
                @click="quantity > 1 && quantity--"
              >-</button>
              <input 
                type="number" 
                class="form-control text-center mx-2" 
                v-model="quantity"
                min="1"
              >
              <button 
                class="btn btn-outline-primary px-3"
                @click="quantity++"
              >+</button>
            </div>
          </div>
  
          <!-- Boutons d'action -->
          <div class="d-grid gap-2">
            <button 
              class="btn btn-primary btn-lg"
              @click="addToCart"
            >
              Ajouter au panier
            </button>
            <button 
              class="btn btn-outline-primary"
              @click="buyNow"
            >
              Acheter maintenant
            </button>
          </div>
  
          <!-- Caractéristiques -->
          <div class="mt-5">
            <h3>Caractéristiques</h3>
            <ul class="list-unstyled mt-3">
              <li v-for="(feature, index) in currentProduct.features" 
                  :key="index"
                  class="mb-2"
              >
                {{ feature }}
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { useCartStore } from '../stores/cartStore'
  import casqueImage from '../img/casque.jpg'
  import iphoneProImage from '../img/iphone-pro.jpg'
  import laptopProImage from '../img/laptop-pro.jpg'
  
  export default {
    name: 'ProductView',
    setup() {
      const cartStore = useCartStore()
      return { cartStore }
    },
    data() {
      return {
        quantity: 1,
        showNotification: false,
        products: [
          {
            id: 1,
            name: 'Casque Audio',
            description: 'Casque bluetooth avec réduction de bruit',
            price: 199.99,
            image: casqueImage,
            category_id: 2,
            features: [
              'Réduction de bruit active',
              'Autonomie de 20 heures',
              'Bluetooth 5.0',
              'Compatible tous appareils',
              'Microphone intégré'
            ]
          },
          {
            id: 2,
            name: 'Laptop Pro',
            description: 'Ordinateur portable puissant pour les professionnels',
            price: 1299.99,
            image: laptopProImage,
            category_id: 1,
            features: [
              'Processeur Intel Core i9',
              'RAM 32 Go',
              'SSD 1 To',
              'Écran 15.6" 4K',
              'Carte graphique RTX 3080'
            ]
          },
          {
            id: 3,
            name: 'iPhone Pro',
            description: 'iPhone haut de gamme avec les dernières technologies',
            price: 1099.99,
            image: iphoneProImage,
            category_id: 1,
            features: [
              'Écran Super Retina XDR',
              'Puce A17 Pro',
              'Triple appareil photo',
              'Face ID',
              'Compatible 5G'
            ]
          }
        ]
      }
    },
    computed: {
      currentProduct() {
        const id = parseInt(this.$route.params.id)
        const product = this.products.find(p => p.id === id)
        return product || this.products[0]
      }
    },
    methods: {
      addToCart() {
        this.cartStore.addToCart(this.currentProduct, this.quantity)
        this.showNotification = true
        setTimeout(() => {
          this.showNotification = false
        }, 3000)
      },
      buyNow() {
        this.addToCart()
        this.$router.push('/checkout')
      }
    }
  }
  </script>
  
  <style scoped>
  .notification {
    position: fixed;
    top: 20px;
    right: 20px;
    background-color: #1a1a1a;
    color: white;
    padding: 1rem;
    border-radius: 8px;
    display: flex;
    align-items: center;
    transform: translateX(150%);
    transition: transform 0.3s ease-in-out;
    z-index: 1000;
    gap: 10px;
  }
  
  .notification.show {
    transform: translateX(0);
  }
  
  input[type="number"] {
    width: 50px;
    text-align: center;
  }
  
  input[type="number"]::-webkit-inner-spin-button,
  input[type="number"]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  
  .btn-primary {
    background-color: #0d6efd;
    border-color: #0d6efd;
  }
  
  .text-primary {
    color: #0d6efd !important;
  }
  
  /* Style pour les caractéristiques */
  ul li {
    padding: 8px 0;
    border-bottom: 1px solid #eee;
  }
  
  ul li:last-child {
    border-bottom: none;
  }
  </style>