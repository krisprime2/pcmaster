<template>
  <v-container fluid class="pa-8">
    <v-row justify="center">
      <v-col cols="12" lg="10" xl="8">
        <h1 class="text-h4 text-white text-center mb-8">Meistverkaufte Produkte</h1>
        <v-row justify="center">
          <v-col v-for="product in topSelling" :key="product.id" cols="12" sm="6" md="4">
            <v-card class="bg-grey-darken-4" elevation="10" height="100%">
              <div class="position-relative">
                <v-img
                    :src="product.image"
                    :alt="product.altText"
                    height="150"
                    cover
                    class="bg-grey-darken-3"
                ></v-img>
              </div>
              <v-card-text>
                <div class="text-subtitle-2 text-grey-lighten-1 mb-1">
                  Modell: {{ product.model }}
                </div>
                <div class="text-body-2 text-grey-lighten-2 mb-3">
                  Preis: {{ product.price }} €
                </div>
                <v-btn
                    color="indigo-accent-2"
                    variant="flat"
                    block
                    class="shop-now-btn"
                    @click="router.push('/articles')"
                >
                  Jetzt Kaufen
                </v-btn>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>

        <h1 class="text-h4 text-white text-center my-8">Neue Produkte</h1>
        <v-row justify="center">
          <v-col v-for="product in newArrivals" :key="product.id" cols="12" sm="6" md="4">
            <v-card class="bg-grey-darken-4" elevation="10" height="100%">
              <v-img
                  :src="product.image"
                  :alt="product.altText"
                  height="150"
                  cover
                  class="bg-grey-darken-3"
              ></v-img>
              <v-card-text>
                <div class="text-h6 text-white mb-2">{{ product.name }}</div>
                <div class="text-body-2 text-grey-lighten-2 mb-2">
                  Price: ${{ product.price }}
                </div>
                <div class="text-body-2 text-grey-lighten-1 mb-3">
                  {{ product.description }}
                </div>
                <v-btn
                    color="indigo-accent-2"
                    variant="flat"
                    block
                    class="shop-now-btn"
                >
                  Jetzt Kaufen
                </v-btn>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import router from "@/router/index.js";
import { BASE_IMAGE_URL } from '@/main.js';

const topSelling = [
  {
    id: 1,
    model: 'XPS 17',
    price: '1,799',
    discount: 15,
    image: BASE_IMAGE_URL + 'assets/images/delllaptop.webp',
    altText: 'Dell XPS 17 Laptop - Hochleistungs-Notebook für professionelle Anwendungen'
  },
  {
    id: 2,
    model: 'Alienware Aurora',
    price: '2,299',
    discount: 20,
    image: BASE_IMAGE_URL + 'assets/images/pcred1.webp',
    altText: 'Alienware Aurora Gaming-PC - Leistungsstarker Gaming-Desktop in Rot'
  },
  {
    id: 3,
    model: 'MacBook Pro',
    price: '2,499',
    discount: 10,
    image: BASE_IMAGE_URL + 'assets/images/macbook1.webp',
    altText: 'MacBook Pro - Professionelles Apple Laptop für kreative Anwendungen'
  }
]

const newArrivals = [
  {
    id: 1,
    name: 'Gaming Beast',
    price: '1,299',
    description: 'High-performance gaming PC',
    image: BASE_IMAGE_URL + 'assets/images/pcblue1.webp',
    altText: 'Gaming Beast - High-Performance Gaming-PC in Blau'
  },
  {
    id: 2,
    name: 'Ultrabook Pro',
    price: '999',
    description: 'Lightweight ultrabook with 16GB RAM',
    image: BASE_IMAGE_URL + 'assets/images/delllaptop2.webp',
    altText: 'Ultrabook Pro - Schlankes Laptop mit 16GB RAM für mobile Produktivität'
  },
  {
    id: 3,
    name: 'Workstation Pro',
    price: '2,499',
    description: 'Workstation with Intel Xeon processor',
    image: BASE_IMAGE_URL + 'assets/images/pcred1.webp',
    altText: 'Workstation Pro - Leistungsstarke Workstation mit Intel Xeon Prozessor'
  }
]
</script>

<style scoped>
.discount-badge {
  position: absolute;
  top: 8px;
  left: 8px;
  background: rgba(var(--v-theme-primary), 0.9);
  color: white;
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 0.8rem;
  font-weight: 500;
}

.shop-now-btn {
  text-transform: none;
  letter-spacing: 0.5px;
}

.v-card {
  transition: transform 0.2s ease-in-out;
}

.v-card:hover {
  transform: translateY(-4px);
}
</style>
