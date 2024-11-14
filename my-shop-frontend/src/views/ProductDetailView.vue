<template>
  <div class="product-detail">
    <nav aria-label="breadcrumb" class="mb-4">
      <ol class="breadcrumb">
        <li class="breadcrumb-item">
          <router-link to="/">Accueil</router-link>
        </li>
        <li class="breadcrumb-item">
          <a href="#" @click.prevent>{{ product.category?.name || 'Catégorie' }}</a>
        </li>
        <li class="breadcrumb-item active" aria-current="page">{{ product.name }}</li>
      </ol>
    </nav>

    <div class="card border-0 shadow-sm">
      <div class="row g-0">
        <div class="col-md-6">
          <div class="product-image p-4">
            <img
              :src="product.image || 'https://via.placeholder.com/600'"
              class="img-fluid rounded"
              :alt="product.name"
            />
          </div>
        </div>
        <div class="col-md-6">
          <div class="card-body p-4">
            <h1 class="card-title mb-4">{{ product.name }}</h1>
            <div class="price-tag mb-4">
              <span class="current-price">{{ product.price }} €</span>
              <span v-if="product.oldPrice" class="old-price ms-2">{{ product.oldPrice }} €</span>
            </div>

            <p class="card-text mb-4">{{ product.description }}</p>

            <div class="product-actions">
              <div class="quantity-selector mb-3">
                <label class="form-label">Quantité</label>
                <div class="input-group" style="width: 150px">
                  <button class="btn btn-outline-primary" @click="quantity > 1 && quantity--">
                    -
                  </button>
                  <input
                    type="number"
                    class="form-control text-center"
                    v-model="quantity"
                    min="1"
                  />
                  <button class="btn btn-outline-primary" @click="quantity++">+</button>
                </div>
              </div>

              <div class="d-grid gap-2">
                <button class="btn btn-primary btn-lg" @click="addToCart">Ajouter au panier</button>
                <button class="btn btn-outline-primary" @click="buyNow">Acheter maintenant</button>
              </div>
            </div>

            <!-- Caractéristiques du produit -->
            <div class="product-features mt-5">
              <h4 class="mb-3">Caractéristiques</h4>
              <ul class="list-unstyled">
                <li v-for="(feature, index) in product.features" :key="index" class="mb-2">
                  <i class="bi bi-check2-circle text-success me-2"></i>
                  {{ feature }}
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Produits similaires -->
    <div class="similar-products mt-5">
      <h3 class="mb-4">Produits similaires</h3>
      <div class="row g-4">
        <div v-for="product in similarProducts" :key="product.id" class="col-6 col-md-3">
          <div class="card h-100">
            <img
              :src="product.image || 'https://via.placeholder.com/300'"
              class="card-img-top p-2"
              :alt="product.name"
            />
            <div class="card-body">
              <h5 class="card-title">{{ product.name }}</h5>
              <p class="card-text text-primary fw-bold">{{ product.price }} €</p>
              <button class="btn btn-outline-primary w-100" @click="goToProduct(product.id)">
                Voir le produit
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Toast de notification -->
    <div class="toast-container position-fixed bottom-0 end-0 p-3">
      <div
        class="toast"
        :class="{ show: showToast }"
        role="alert"
        aria-live="assertive"
        aria-atomic="true"
      >
        <div class="toast-header">
          <strong class="me-auto">MyShop</strong>
          <button type="button" class="btn-close" @click="showToast = false"></button>
        </div>
        <div class="toast-body">
          {{ toastMessage }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { useCartStore } from '@/stores/cartStore'
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

export default {
  name: 'ProductDetailView',
  setup() {
    const cartStore = useCartStore()
    const route = useRoute()
    const router = useRouter()
    const quantity = ref(1)
    const showToast = ref(false)
    const toastMessage = ref('')

    return {
      cartStore,
      route,
      router,
      quantity,
      showToast,
      toastMessage,
    }
  },
  data() {
    return {
      product: {
        id: null,
        name: '',
        description: '',
        price: 0,
        image: '',
        features: [],
        category: null,
      },
      similarProducts: [],
    }
  },
  methods: {
    addToCart() {
      const productWithQuantity = {
        ...this.product,
        quantity: this.quantity,
      }
      this.cartStore.addToCart(productWithQuantity)
      this.toastMessage = `${this.quantity} ${this.product.name} ajouté(s) au panier`
      this.showToast = true
      setTimeout(() => {
        this.showToast = false
      }, 3000)
    },
    buyNow() {
      this.addToCart()
      this.router.push('/checkout')
    },
    goToProduct(productId) {
      this.router.push(`/products/${productId}`)
    },
    async fetchProduct() {
      // Simuler un appel API
      this.product = {
        id: this.route.params.id,
        name: 'Smartphone XYZ',
        description:
          'Un super smartphone avec plein de fonctionnalités incroyables. Design élégant, performances exceptionnelles.',
        price: 499.99,
        oldPrice: 599.99,
        image: 'https://via.placeholder.com/600',
        features: [
          'Écran 6.5" AMOLED',
          'Processeur dernière génération',
          'Appareil photo 48MP',
          'Batterie 5000mAh',
          'Charge rapide 65W',
        ],
        category: { id: 1, name: 'Électronique' },
      }

      // Simuler des produits similaires
      this.similarProducts = [
        { id: 2, name: 'Smartphone ABC', price: 399.99 },
        { id: 3, name: 'Smartphone DEF', price: 599.99 },
        { id: 4, name: 'Smartphone GHI', price: 449.99 },
        { id: 5, name: 'Smartphone JKL', price: 649.99 },
      ]
    },
  },
  onMounted() {
    this.fetchProduct()
  },
}
</script>

<style scoped>
.product-image {
  background-color: #f8f9fa;
  border-radius: 12px;
}

.current-price {
  font-size: 2rem;
  font-weight: bold;
  color: #4361ee;
}

.old-price {
  font-size: 1.2rem;
  text-decoration: line-through;
  color: #6c757d;
}

.product-features li {
  display: flex;
  align-items: center;
}

.quantity-selector input {
  text-align: center;
  /* Remplacer -moz-appearance par appearance qui est standard */
  appearance: textfield;
}

/* Masquer les flèches pour Chrome, Safari, Edge et Opera */
.quantity-selector input::-webkit-outer-spin-button,
.quantity-selector input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}.similar-products .card {
  transition: transform 0.2s;
}

.similar-products .card:hover {
  transform: translateY(-5px);
}

.toast {
  transition: opacity 0.3s ease;
}
</style>
