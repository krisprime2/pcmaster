<template>
  <v-container>
    <v-card class="pa-4">
      <h1 class="text-h3 text-center my-6">Warenkorb</h1>
      <v-data-table
          :headers="headers"
          :items="cart"
          item-value="product.id"
          class="elevation-1"
          dense
          no-data-text="Der Warenkorb ist leer"
      >
        <template #item.type="{ item }">
          <v-chip color="primary" outlined small>
            {{ item.type === 'configuration' ? 'Konfiguration' : 'Artikel' }}
          </v-chip>
        </template>

        <template #item.name="{ item }">
          <div class="d-flex align-center">
            <v-avatar class="me-2">
              <v-img :src="item.type === 'configuration' ? '/images/config-icon.png' : item.item.imageUrl" alt="Product Image"></v-img>
            </v-avatar>
            <span>{{ item.type === 'configuration' ? 'Deine PC Konfiguration' : item.item.name }}</span>
          </div>
        </template>

        <template #item.price="{ item }">
          {{ item.item.price.toFixed(2) }} €
        </template>

        <template #item.quantity="{ item }">
          <v-text-field
              v-model.number="item.quantity"
              type="number"
              min="1"
              dense
              hide-details
              class="w-50"
              @change="updateQuantity(item)"
          ></v-text-field>
        </template>

        <template #item.total="{ item }">
          {{ (item.item.price * item.quantity).toFixed(2) }} €
        </template>

        <template #item.actions="{ item }">
          <v-icon
              color="red darken-1"
              clickable
              @click="removeItem(item.item.id, item.type)"
              title="Entfernen"
          >
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>

      <v-divider class="my-4"></v-divider>

      <div class="d-flex justify-space-between">
        <h2 class="text-h6">Gesamt: {{ cartTotal.toFixed(2) }} €</h2>
        <v-btn
            color="green darken-1"
            :disabled="isCartEmpty"
            @click="goToCheckout"
        >
          Zur Kasse
        </v-btn>
      </div>
    </v-card>
  </v-container>
</template>

<script setup>
import {ref, onMounted, computed} from 'vue';
import axios from "axios";
import router from "@/router/index.js";

const cart = ref([]);
const headers = [
  { text: "Typ", value: "type" },
  { text: "Produkt", value: "name" },
  { text: "Preis", value: "price", align: "end" },
  { text: "Menge", value: "quantity", align: "center" },
  { text: "Gesamt", value: "total", align: "end" },
  { text: "Aktionen", value: "actions", align: "center" },
];
const isCartEmpty = computed(() => cart.value.length === 0);

const cartTotal = computed(() =>
    cart.value.reduce((sum, item) => sum + item.item.price * item.quantity, 0)
);
function goToCheckout() {
  if (!isCartEmpty.value) {
    router.push('/checkout');
  }
}

async function fetchCart() {
  try {
    const response = await axios.get('/api/cart');
    cart.value = response.data.map(cartItem => ({
      ...cartItem,
      item: cartItem.item,
      type: cartItem.type,
      quantity: cartItem.quantity
    }));
    console.log('Fetched cart:', cart.value);
  } catch (error) {
    console.error('Error fetching cart:', error);
  }
}

async function removeItem(itemId, type) {
  try {
    console.log('Removing item:', { itemId, type });
    await axios.post('/api/cart/remove', { itemId, type });
    await fetchCart();
  } catch (error) {
    console.error('Error removing item from cart:', error.response?.data || error.message);
  }
}

async function clearCart() {
  await axios.post(`/api/cart/clear`)
  await fetchCart();
}

onMounted(fetchCart);
</script>

<style scoped>
.v-avatar {
  width: 40px;
  height: 40px;
}
</style>
