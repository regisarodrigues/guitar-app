<script setup>
import { computed } from 'vue';

const props = defineProps({
  cart: {
    type: Array,
    required: true
  },
  guitar: {
    type: Object,
    required: true
  }
});
defineEmits(['increase-amount', 'decrease-amount', 'increase-cart', 'dell-product', 'dell-cart']);

const totalAmount = computed(() => {
  return props.cart.reduce((total, product) => total + product.amount * product.precio, 0);
});
</script>

<template>
  <header class="py-5 header">
    <div class="container-xl">
      <div class="row justify-content-center justify-content-md-between">
        <div class="col-8 col-md-3">
          <a href="index.html">
            <img class="img-fluid" src="/img/logo.svg" alt="imagen logo" />
          </a>
        </div>
        <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
          <div class="carrito">
            <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />

            <div id="carrito" class="bg-white p-3">
              <p class="text-center" v-if="cart.length === 0">Carrinho está vazio.</p>
              <div v-else>
                <table class="w-100 table">
                  <thead>
                    <tr>
                      <th>Imagem</th>
                      <th>Nome</th>
                      <th>Preço</th>
                      <th>Quantidade</th>
                      <th></th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="guitar in cart" :key="guitar.id">
                      <td>
                        <img
                          class="img-fluid"
                          :src="'/img/' + guitar.imagen + '.jpg'"
                          :alt="'imagen guitarra' + guitar.nombre"
                        />
                      </td>
                      <td>{{ guitar.nombre }}</td>
                      <td class="fw-bold">${{ guitar.precio }}</td>
                      <td class="flex align-items-start gap-4">
                        <button
                          type="button"
                          class="btn btn-dark"
                          @click="$emit('decrease-amount', guitar.id)"
                        >
                          -
                        </button>
                        {{ guitar.amount }}
                        <button
                          type="button"
                          class="btn btn-dark"
                          @click="$emit('increase-amount', guitar.id)"
                        >
                          +
                        </button>
                      </td>
                      <td>
                        <button
                          type="button"
                          class="btn btn-danger"
                          @click="$emit('dell-product', guitar.id)"
                        >
                          X
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>

                <p class="text-end">
                  Total pagar: <span class="fw-bold">${{ totalAmount }}</span>
                </p>
                <button class="btn btn-dark w-100 mt-3 p-2" @click="$emit('dell-cart')">
                  Esvaziar Carrinho
                </button>
              </div>
            </div>
          </div>
        </nav>
      </div>
      <!--.row-->

      <div class="row mt-5">
        <div class="col-md-6 text-center text-md-start pt-5">
          <h1 class="display-2 fw-bold">Modelo {{ guitar.nombre }}</h1>
          <p class="mt-5 fs-5 text-white">
            {{ guitar.descripcion }}
          </p>
          <p class="text-primary fs-1 fw-black">${{ guitar.precio }}</p>
          <button
            type="button"
            class="btn fs-4 bg-primary text-white py-2 px-5"
            @click="$emit('increase-cart', guitar)"
          >
            Adicionar ao Carrinho
          </button>
        </div>
      </div>
    </div>

    <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header" />
  </header>
</template>
