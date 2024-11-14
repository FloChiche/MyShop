<template>
  <div class="categories-admin">
    <div class="d-flex justify-content-between align-items-center mb-4">
      <h2>Gestion des catégories</h2>
      <button class="btn btn-primary" @click="showAddModal = true">Ajouter une catégorie</button>
    </div>

    <!-- Table des catégories -->
    <div class="table-responsive">
      <table class="table table-striped">
        <thead>
          <tr>
            <th>ID</th>
            <th>Nom</th>
            <th>Description</th>
            <th>Nombre de produits</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="category in categories" :key="category.id">
            <td>{{ category.id }}</td>
            <td>{{ category.name }}</td>
            <td>{{ category.description }}</td>
            <td>{{ category.productCount }}</td>
            <td>
              <button class="btn btn-sm btn-outline-primary me-1" @click="editCategory(category)">
                Modifier
              </button>
              <button class="btn btn-sm btn-outline-danger" @click="deleteCategory(category.id)">
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
            <h5 class="modal-title">{{ editMode ? 'Modifier' : 'Ajouter' }} une catégorie</h5>
            <button type="button" class="btn-close" @click="closeModal"></button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="saveCategory">
              <div class="mb-3">
                <label class="form-label">Nom</label>
                <input type="text" class="form-control" v-model="currentCategory.name" required />
              </div>
              <div class="mb-3">
                <label class="form-label">Description</label>
                <textarea
                  class="form-control"
                  v-model="currentCategory.description"
                  rows="3"
                ></textarea>
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
  name: 'CategoriesView',
  data() {
    return {
      categories: [
        // Données de test
        {
          id: 1,
          name: 'Électronique',
          description: 'Produits électroniques',
          productCount: 15,
        },
        {
          id: 2,
          name: 'Vêtements',
          description: 'Vêtements et accessoires',
          productCount: 25,
        },
      ],
      showAddModal: false,
      editMode: false,
      currentCategory: {
        name: '',
        description: '',
      },
    }
  },
  methods: {
    editCategory(category) {
      this.currentCategory = { ...category }
      this.editMode = true
      this.showAddModal = true
    },
    deleteCategory(id) {
      if (confirm('Êtes-vous sûr de vouloir supprimer cette catégorie ?')) {
        // Ici nous ajouterons l'appel API pour la suppression
        this.categories = this.categories.filter((c) => c.id !== id)
      }
    },
    saveCategory() {
      // Ici nous ajouterons l'appel API pour la sauvegarde
      if (this.editMode) {
        const index = this.categories.findIndex((c) => c.id === this.currentCategory.id)
        this.categories[index] = { ...this.currentCategory }
      } else {
        this.categories.push({
          id: Date.now(), // Simulé pour le test
          productCount: 0,
          ...this.currentCategory,
        })
      }
      this.closeModal()
    },
    closeModal() {
      this.showAddModal = false
      this.editMode = false
      this.currentCategory = {
        name: '',
        description: '',
      }
    },
  },
}
</script>
