<script lang="ts">
import { defineComponent } from 'vue'
import { Product } from './model/product.model'
import  Cart  from './model/cart.model'
import ProductCard from './components/card/ProductCard.vue'
import type { CartItem } from "./models/cartItem.model"
import type { Category } from "./models/category.model"

export default defineComponent({
    components: { ProductCard },

   data() {
    const gamer = { id: 1, title: "Gamer"};
    return {
    cart: new Cart(),
       products:[ 
        new Product( 1, "Teclado gamer","Teclado mecânico com iluminação RGB", 200, gamer,),
        new Product(2, "Mouse gamer", "Mouse gamer RGB com alta precisão", 150, gamer,),
       ],
        cartItems: [] as CartItem[]
     }
  },
   methods: {

    addItem(product: Product) {
      this.cart.addItem(product)
    },

    decItem(id: number) {
      this.cart.decItem(id)
    },

    excludeItem(name: string) {
      this.cart.excludeItem(name)
    },

  },

  computed: {

    totalItems(): number {
      return this.cartItems.reduce(
        (total, item) => total + item.quantity,
        0
      )
    },
    totalPrice(): number {
      return this.cartItems.reduce(
        (total, item) => total + item.product.price * item.quantity,
        0
      )
    }

  }

})
</script>

<template>
  <main style="padding: 20px;">
    <ProductCard
       v-for="product in products"
      :key="product.id"
      :product="product"
      @addItem="addItem" />

    <hr> 

    <section class="cart-summary">
      <h1>Carrinho</h1>
      
      <div v-if="cart.list.length > 0">
        <div v-for="(item, index) in cart.list" :key="index">
          <p> 
            <strong>{{ item.product.name }}</strong> 
            <br> 
            Quantidade: {{ item.quantity }} - R$: {{ item.product.price * item.quantity }},00
          </p> 
          
          <button @click="decItem(item.product.id)"> Remover quantidade </button>

          <button @click="excludeItem(item.product.name)" style="color: red; margin-left: 10px;"> 
            Excluir todos os itens 
          </button>
          
          <hr>
        </div>

        <p><strong>Total de itens:</strong> {{ cart.total }}</p>
        <p><strong>Valor da Compra: R$ {{ cart.price }},00</strong></p>
      </div>

      <p v-else>O carrinho está vazio.</p>
    </section>
  </main>
</template>