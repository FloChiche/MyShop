<template>
  <div class="home p-4">
    <h1 class="mb-5" style="font-weight: normal">Bienvenue sur MyShop</h1>

    <!-- Filtres -->
    <div class="d-flex gap-3 mb-5">
      <input
        type="text"
        class="form-control search-input"
        placeholder="Rechercher un produit"
        v-model="searchQuery"
      />
      <select class="form-select" v-model="selectedCategory">
        <option value="">Toutes les catégories</option>
        <option v-for="category in categories" :key="category.id" :value="category.id">
          {{ category.name }}
        </option>
      </select>
      <select class="form-select" v-model="sortBy">
        <option value="name">Trier par nom</option>
        <option value="price-asc">Prix croissant</option>
        <option value="price-desc">Prix décroissant</option>
      </select>
    </div>

    <!-- Grille de produits -->
    <div class="row g-4">
      <div v-for="product in filteredProducts" :key="product.id" class="col-12 col-md-6 col-lg-4">
        <div class="card border">
          <div class="card-body p-4">
            <img :src="product.image" class="img-fluid mb-3 w-100" :alt="product.name" />
            <h5 class="product-title">{{ product.name }}</h5>
            <p class="product-description">{{ product.description }}</p>
            <p class="product-price">{{ product.price }} €</p>
            <div class="d-flex justify-content-between align-items-center">
              <router-link
                :to="'/products/' + product.id"
                class="btn btn-link text-primary text-decoration-none p-0"
              >
                Voir détails
              </router-link>
              <button class="btn btn-primary rounded" @click="addToCart(product)">+</button>
            </div>
          </div>
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
  name: 'HomeView',
  setup() {
    const cartStore = useCartStore()
    return { cartStore }
  },
  data() {
    return {
      products: [
        {
          id: 1,
          name: 'Casque Audio',
          description: 'Casque bluetooth avec réduction de bruit',
          price: 199.99,
          image: casqueImage,
          category_id: 2,
        },
        {
          id: 2,
          name: 'Laptop Pro',
          description: 'Ordinateur portable puissant pour les professionnels',
          price: 1299.99,
          image: laptopProImage,
          category_id: 1,
        },
        {
          id: 3,
          name: 'iPhone Pro',
          description: 'iPhone haut de gamme avec les dernières technologies',
          price: 1099.99,
          image: iphoneProImage,
          category_id: 1,
        },
      ],
      categories: [
        { id: 1, name: 'Électronique' },
        { id: 2, name: 'Audio' },
      ],
      searchQuery: '',
      selectedCategory: '',
      sortBy: 'name',
    }
  },
  computed: {
    filteredProducts() {
      let filtered = [...this.products]

      if (this.searchQuery) {
        filtered = filtered.filter(
          (product) =>
            product.name.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
            product.description.toLowerCase().includes(this.searchQuery.toLowerCase()),
        )
      }

      if (this.selectedCategory) {
        filtered = filtered.filter((product) => product.category_id === this.selectedCategory)
      }

      filtered.sort((a, b) => {
        switch (this.sortBy) {
          case 'price-asc':
            return a.price - b.price
          case 'price-desc':
            return b.price - a.price
          default:
            return a.name.localeCompare(b.name)
        }
      })

      return filtered
    },
  },
  methods: {
    addToCart(product) {
      this.cartStore.addToCart(product)
      alert('Produit ajouté au panier !')
    },
  },
}
</script>

<style scoped>
.search-input {
  flex-grow: 1;
}

.form-control,
.form-select {
  border-radius: 8px;
  border: 1px solid #dee2e6;
  padding: 0.5rem 1rem;
}

.card {
  border-radius: 12px;
}

.product-title {
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
}

.product-description {
  color: #6c757d;
  font-size: 0.9rem;
  margin-bottom: 1rem;
}

.product-price {
  color: #0d6efd;
  font-size: 1.2rem;
  margin-bottom: 1rem;
}

.btn-primary {
  background-color: #0d6efd;
  border-color: #0d6efd;
  width: 35px;
  height: 35px;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
}
</style>
