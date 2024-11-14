<template>
  <div class="cart-sidebar" :class="{ show: isOpen }">
    <div class="cart-header">
      <h5>Mon Panier ({{ cartStore.totalItems }})</h5>
      <button class="btn-close" @click="close"></button>
    </div>

    <div v-if="cartStore.items.length === 0" class="cart-empty">
      <i class="bi bi-cart3 display-4"></i>
      <p>Votre panier est vide</p>
      <button class="btn btn-primary" @click="close">Continuer mes achats</button>
    </div>

    <div v-else class="cart-content">
      <div class="cart-items">
        <div v-for="item in cartStore.items" :key="item.id" class="cart-item">
          <img
            :src="item.image || 'https://via.placeholder.com/80'"
            :alt="item.name"
            class="item-image"
          />
          <div class="item-details">
            <h6 class="item-title">{{ item.name }}</h6>
            <p class="item-price">{{ item.price }} €</p>
            <div class="item-quantity">
              <button
                class="btn btn-sm btn-outline-secondary"
                @click="updateQuantity(item, item.quantity - 1)"
                :disabled="item.quantity <= 1"
              >
                -
              </button>
              <span>{{ item.quantity }}</span>
              <button
                class="btn btn-sm btn-outline-secondary"
                @click="updateQuantity(item, item.quantity + 1)"
              >
                +
              </button>
            </div>
          </div>
          <button class="btn btn-sm btn-outline-danger remove-item" @click="removeItem(item.id)">
            <i class="bi bi-trash"></i>
          </button>
        </div>
      </div>

      <div class="cart-summary">
        <div class="summary-item">
          <span>Sous-total</span>
          <span>{{ cartStore.totalPrice.toFixed(2) }} €</span>
        </div>
        <div class="summary-item">
          <span>Livraison</span>
          <span>{{ shippingCost.toFixed(2) }} €</span>
        </div>
        <div class="summary-item total">
          <span>Total</span>
          <span>{{ (cartStore.totalPrice + shippingCost).toFixed(2) }} €</span>
        </div>

        <router-link to="/checkout" class="btn btn-primary w-100 mb-2" @click="close">
          Passer la commande
        </router-link>
        <button class="btn btn-outline-primary w-100" @click="close">Continuer mes achats</button>
      </div>
    </div>
  </div>

  <!-- Overlay -->
  <div v-if="isOpen" class="cart-overlay" @click="close"></div>
</template>

<script>
import { useCartStore } from '@/stores/cartStore'

export default {
  name: 'CartSidebar',
  props: {
    isOpen: {
      type: Boolean,
      default: false,
    },
  },
  setup() {
    const cartStore = useCartStore()
    return { cartStore }
  },
  data() {
    return {
      shippingCost: 5.99,
    }
  },
  methods: {
    close() {
      this.$emit('update:isOpen', false)
    },
    updateQuantity(item, newQuantity) {
      if (newQuantity < 1) return
      this.cartStore.updateQuantity(item.id, newQuantity)
    },
    removeItem(productId) {
      if (confirm('Voulez-vous vraiment retirer cet article du panier ?')) {
        this.cartStore.removeFromCart(productId)
      }
    },
  },
}
</script>

<style scoped>
.cart-sidebar {
  position: fixed;
  top: 0;
  right: -400px;
  width: 400px;
  height: 100vh;
  background: white;
  box-shadow: -2px 0 5px rgba(0, 0, 0, 0.1);
  z-index: 1050;
  transition: right 0.3s ease;
  display: flex;
  flex-direction: column;
}

.cart-sidebar.show {
  right: 0;
}

.cart-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1040;
}

.cart-header {
  padding: 1rem;
  border-bottom: 1px solid #e0e0e0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cart-empty {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  color: #6c757d;
}

.cart-content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.cart-items {
  flex: 1;
  overflow-y: auto;
  padding: 1rem;
}

.cart-item {
  display: flex;
  gap: 1rem;
  padding: 1rem;
  border-bottom: 1px solid #e0e0e0;
  position: relative;
}

.item-image {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 8px;
}

.item-details {
  flex: 1;
}

.item-title {
  margin: 0 0 0.5rem 0;
}

.item-price {
  color: #4361ee;
  font-weight: bold;
  margin: 0;
}

.item-quantity {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-top: 0.5rem;
}

.item-quantity span {
  min-width: 30px;
  text-align: center;
}

.remove-item {
  position: absolute;
  top: 1rem;
  right: 1rem;
}

.cart-summary {
  padding: 1rem;
  border-top: 1px solid #e0e0e0;
  background: #f8f9fa;
}

.summary-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.5rem;
}

.summary-item.total {
  font-weight: bold;
  font-size: 1.2rem;
  margin: 1rem 0;
  padding-top: 1rem;
  border-top: 1px solid #e0e0e0;
}

@media (max-width: 576px) {
  .cart-sidebar {
    width: 100%;
    right: -100%;
  }
}
</style>
