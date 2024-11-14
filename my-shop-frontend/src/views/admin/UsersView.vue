<template>
    <div class="users-admin">
      <div class="d-flex justify-content-between align-items-center mb-4">
        <h2>Gestion des utilisateurs</h2>
        <button 
          class="btn btn-primary" 
          @click="showAddModal = true"
        >
          Ajouter un utilisateur
        </button>
      </div>
  
      <!-- Table des utilisateurs -->
      <div class="table-responsive">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>ID</th>
              <th>Nom</th>
              <th>Email</th>
              <th>Rôle</th>
              <th>Date d'inscription</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in users" :key="user.id">
              <td>{{ user.id }}</td>
              <td>{{ user.username }}</td>
              <td>{{ user.email }}</td>
              <td>
                <span 
                  class="badge"
                  :class="user.role === 'admin' ? 'bg-danger' : 'bg-success'"
                >
                  {{ user.role }}
                </span>
              </td>
              <td>{{ formatDate(user.createdAt) }}</td>
              <td>
                <button 
                  class="btn btn-sm btn-outline-primary me-1"
                  @click="editUser(user)"
                >
                  Modifier
                </button>
                <button 
                  class="btn btn-sm btn-outline-danger"
                  @click="deleteUser(user.id)"
                  :disabled="user.role === 'admin'"
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
              <h5 class="modal-title">{{ editMode ? 'Modifier' : 'Ajouter' }} un utilisateur</h5>
              <button 
                type="button" 
                class="btn-close" 
                @click="closeModal"
              ></button>
            </div>
            <div class="modal-body">
              <form @submit.prevent="saveUser">
                <div class="mb-3">
                  <label class="form-label">Nom d'utilisateur</label>
                  <input 
                    type="text" 
                    class="form-control" 
                    v-model="currentUser.username" 
                    required
                  >
                </div>
                <div class="mb-3">
                  <label class="form-label">Email</label>
                  <input 
                    type="email" 
                    class="form-control" 
                    v-model="currentUser.email" 
                    required
                  >
                </div>
                <div class="mb-3">
                  <label class="form-label">Rôle</label>
                  <select 
                    class="form-select" 
                    v-model="currentUser.role"
                  >
                    <option value="user">Utilisateur</option>
                    <option value="admin">Administrateur</option>
                  </select>
                </div>
                <div class="mb-3" v-if="!editMode">
                  <label class="form-label">Mot de passe</label>
                  <input 
                    type="password" 
                    class="form-control" 
                    v-model="currentUser.password"
                    :required="!editMode"
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
    name: 'UsersView',
    data() {
      return {
        users: [
          // Données de test
          {
            id: 1,
            username: 'admin',
            email: 'admin@example.com',
            role: 'admin',
            createdAt: '2024-01-01'
          },
          {
            id: 2,
            username: 'user1',
            email: 'user1@example.com',
            role: 'user',
            createdAt: '2024-01-02'
          }
        ],
        showAddModal: false,
        editMode: false,
        currentUser: {
          username: '',
          email: '',
          role: 'user',
          password: ''
        }
      }
    },
    methods: {
      formatDate(date) {
        return new Date(date).toLocaleDateString('fr-FR')
      },
      editUser(user) {
        this.currentUser = { 
          ...user,
          password: '' // On ne récupère jamais le mot de passe
        }
        this.editMode = true
        this.showAddModal = true
      },
      deleteUser(id) {
        if (confirm('Êtes-vous sûr de vouloir supprimer cet utilisateur ?')) {
          // Ici nous ajouterons l'appel API pour la suppression
          this.users = this.users.filter(u => u.id !== id)
        }
      },
      saveUser() {
        // Ici nous ajouterons l'appel API pour la sauvegarde
        if (this.editMode) {
          const index = this.users.findIndex(u => u.id === this.currentUser.id)
          this.users[index] = { ...this.currentUser }
        } else {
          this.users.push({
            id: Date.now(), // Simulé pour le test
            createdAt: new Date().toISOString().split('T')[0],
            ...this.currentUser
          })
        }
        this.closeModal()
      },
      closeModal() {
        this.showAddModal = false
        this.editMode = false
        this.currentUser = {
          username: '',
          email: '',
          role: 'user',
          password: ''
        }
      }
    }
  }
  </script>