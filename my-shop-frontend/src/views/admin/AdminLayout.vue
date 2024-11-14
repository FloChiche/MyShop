<template>
  <div class="admin-layout">
    <div class="container-fluid">
      <div class="row">
        <!-- Sidebar -->
        <nav class="col-md-3 col-lg-2 d-md-block bg-light sidebar">
          <div class="position-sticky pt-3">
            <h6 class="sidebar-heading px-3 mt-4 mb-1 text-muted">Administration</h6>
            <ul class="nav flex-column">
              <li class="nav-item">
                <router-link to="/admin/products" class="nav-link" active-class="active">
                  <i class="bi bi-box"></i>
                  Produits
                </router-link>
              </li>
              <li class="nav-item">
                <router-link to="/admin/categories" class="nav-link" active-class="active">
                  <i class="bi bi-tags"></i>
                  Catégories
                </router-link>
              </li>
              <li class="nav-item">
                <router-link to="/admin/users" class="nav-link" active-class="active">
                  <i class="bi bi-people"></i>
                  Utilisateurs
                </router-link>
              </li>
            </ul>
          </div>
        </nav>

        <!-- Main content -->
        <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
          <div
            class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
          >
            <h1 class="h2">Dashboard</h1>
          </div>

          <!-- Router view pour le contenu admin -->
          <router-view></router-view>
        </main>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AdminLayout',
}
</script>

<style scoped>
.sidebar {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  z-index: 100;
  padding: 48px 0 0;
  box-shadow: inset -1px 0 0 rgba(0, 0, 0, 0.1);
}

.sidebar .nav-link {
  font-weight: 500;
  color: #333;
}

.sidebar .nav-link.active {
  color: #2470dc;
}

main {
  padding-top: 48px;
}
</style>
