<template>

  <div class="product-card">

    <h1>{{ product.name }}</h1>
    <p>{{ product.description }}</p>
    <p>R$: {{ product.price }},00</p>

    <button @click="$emit('addItem', product)">Adicionar</button>

  </div>

</template>

<script lang="ts">
import { defineComponent } from "vue"
import type { Product } from "../models/product.model"

export default defineComponent({

  props: {
    product: {
      type: Object as () => Product,
      required: true
    }
  }

})
</script>
