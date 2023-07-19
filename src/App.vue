<script setup>
import FooterComponent from '@/components/FooterComponent.vue';
import GuitarList from '@/components/GuitarList.vue';
import HeaderComponent from '@/components/HeaderComponent.vue';
import { db } from '@/data/guitars';
import { onMounted, ref, watch } from 'vue';

const guitars = ref([]);
const cart = ref([]);
const guitar = ref({});

watch(
  cart,
  () => {
    saveCartLocalStorage();
  },
  {
    deep: true
  }
);

onMounted(() => {
  guitars.value = db;
  guitar.value = db[3];

  const cartStorage = localStorage.getItem('cart');

  if (cartStorage) cart.value = JSON.parse(cartStorage);
});

const saveCartLocalStorage = () => {
  localStorage.setItem('cart', JSON.stringify(cart.value));
};

const increaseCart = (guitar) => {
  const existProduct = cart.value.findIndex((product) => product.id === guitar.id);

  if (existProduct >= 0) {
    cart.value[existProduct].amount++;
  } else {
    guitar.amount = 1;
    cart.value.push(guitar);
  }
};

const increaseAmount = (id) => {
  const index = cart.value.findIndex((product) => product.id === id);
  if (cart.value[index].amount >= 5) return;
  cart.value[index].amount++;
};

const decreaseAmount = (id) => {
  const index = cart.value.findIndex((product) => product.id === id);
  if (cart.value[index].amount <= 1) return;
  cart.value[index].amount--;
};

const dellProduct = (id) => {
  cart.value = cart.value.filter((product) => product.id !== id);
};

const dellCart = () => {
  cart.value = [];
};
</script>

<template>
  <!-- HEADER -->
  <HeaderComponent
    :cart="cart"
    :guitar="guitar"
    @increase-amount="increaseAmount"
    @decrease-amount="decreaseAmount"
    @increase-cart="increaseCart"
    @dell-product="dellProduct"
    @dell-cart="dellCart"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nossa Coleção</h2>

    <!-- GUITARRAS -->
    <div class="row mt-5">
      <GuitarList
        v-for="guitar in guitars"
        :key="guitar.id"
        :guitar="guitar"
        @increase-cart="increaseCart"
      />
    </div>
  </main>

  <!-- FOOTER -->
  <FooterComponent />
</template>
