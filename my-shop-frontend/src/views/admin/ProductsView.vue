<template>
    <div class="products-admin">
      <div class="d-flex justify-content-between align-items-center mb-4">
        <h2>Gestion des produits</h2>
        <button 
          class="btn btn-primary" 
          @click="showAddModal = true"
        >
          Ajouter un produit
        </button>
      </div>
  
      <!-- Table des produits -->
      <div class="table-responsive">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>ID</th>
              <th>Image</th>
              <th>Nom</th>
              <th>Prix</th>
              <th>Catégories</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="product in products" :key="product.id">
              <td>{{ product.id }}</td>
              <td>
                <img 
                  :src="product.image" 
                  alt="Product"
                  style="width: 50px; height: 50px; object-fit: cover"
                >
              </td>
              <td>{{ product.name }}</td>
              <td>{{ product.price }} €</td>
              <td>
                <span 
                  v-for="category in product.categories" 
                  :key="category.id"
                  class="badge bg-secondary me-1"
                >
                  {{ category.name }}
                </span>
              </td>
              <td>
                <button 
                  class="btn btn-sm btn-outline-primary me-1"
                  @click="editProduct(product)"
                >
                  Modifier
                </button>
                <button 
                  class="btn btn-sm btn-outline-danger"
                  @click="deleteProduct(product.id)"
                >
                  Supprimer
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
  
      <!-- Modal d'ajout/modification -->
      <div class="modal" v-if="showAddModal" tabindex="-1" style="display: block">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">{{ editMode ? 'Modifier' : 'Ajouter' }} un produit</h5>
              <button 
                type="button" 
                class="btn-close" 
                @click="closeModal"
              ></button>
            </div>
            <div class="modal-body">
              <form @submit.prevent="saveProduct">
                <div class="mb-3">
                  <label class="form-label">Nom</label>
                  <input 
                    type="text" 
                    class="form-control" 
                    v-model="currentProduct.name" 
                    required
                  >
                </div>
                <div class="mb-3">
                  <label class="form-label">Prix</label>
                  <input 
                    type="number" 
                    class="form-control" 
                    v-model="currentProduct.price" 
                    required
                  >
                </div>
                <div class="mb-3">
                  <label class="form-label">Description</label>
                  <textarea 
                    class="form-control" 
                    v-model="currentProduct.description"
                    rows="3"
                  ></textarea>
                </div>
                <div class="mb-3">
                  <label class="form-label">Image URL</label>
                  <input 
                    type="text" 
                    class="form-control" 
                    v-model="currentProduct.image"
                  >
                </div>
                <button type="submit" class="btn btn-primary">Enregistrer</button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ProductsView',
    data() {
      return {
        products: [
          // Données de test
          {
            id: 1,
            name: 'Produit test',
            price: 99.99,
            description: 'Description test',
            image: 'https://via.placeholder.com/150',
            categories: [{ id: 1, name: 'Catégorie 1' }]
          }
        ],
        showAddModal: false,
        editMode: false,
        currentProduct: {
          name: '',
          price: 0,
          description: '',
          image: ''
        }
      }
    },
    methods: {
      editProduct(product) {
        this.currentProduct = { ...product }
        this.editMode = true
        this.showAddModal = true
      },
      deleteProduct(id) {
        if (confirm('Êtes-vous sûr de vouloir supprimer ce produit ?')) {
          // Ici nous ajouterons l'appel API pour la suppression
          this.products = this.products.filter(p => p.id !== id)
        }
      },
      saveProduct() {
        // Ici nous ajouterons l'appel API pour la sauvegarde
        if (this.editMode) {
          const index = this.products.findIndex(p => p.id === this.currentProduct.id)
          this.products[index] = { ...this.currentProduct }
        } else {
          this.products.push({
            id: Date.now(), // Simulé pour le test
            ...this.currentProduct
          })
        }
        this.closeModal()
      },
      closeModal() {
        this.showAddModal = false
        this.editMode = false
        this.currentProduct = {
          name: '',
          price: 0,
          description: '',
          image: ''
        }
      }
    }
  }
  </script>