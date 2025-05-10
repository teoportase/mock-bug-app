<script>
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    return {
      counter: 0,
      items: [
        { item_name: 'Nachos', item_src: '/src/assets/nachos.jpg' },
        { item_name: 'Pizza', item_src: '/src/assets/pizza.jpg' },
        { item_name: 'Burrito', item_src: '/src/assets/burrito.jpg' },
        { item_name: 'Kebab', item_src: '/src/assets/kebab.jpg' },
        { item_name: 'Falafel', item_src: '/src/assets/falafel.jpg' },
        { item_name: 'Spaghetti', item_src: '/src/assets/spaghetti.jpg' },
        { item_name: 'Burger', item_src: '/src/assets/burger.jpg' },
      ],
      cart: new Map(),
      isVisible: false,
    }
  },
  methods: {
    addToCart(item_name) {
      if (this.counter < 5) {
        this.counter++;
        let hasAmount = this.cart.has(item_name);

        // this is disgusting
        hasAmount ? this.cart.set(item_name, this.cart.get(item_name) + 1) : this.cart.set(item_name, 1);
      } else {
        throw new Error("Index out of bounds");
      }
    },
    removeFromCart(item_name) {
      if (this.cart.has(item_name)) {
        const currentQuantity = this.cart.get(item_name);
        if (currentQuantity > 1) {
          this.cart.set(item_name, currentQuantity - 1);
        } else {
          this.cart.delete(item_name);
        }
        this.counter--;
      }
    },
    showCart() {
      this.isVisible = !this.isVisible;
    },
    checkout() {
      alert("Checkout Successful!");
      location.reload();
    }
  },
});
</script>


<template>
  <div>
    <!-- Title -->
    <div class="banner">
      <h1> Vydra Shopping </h1>
    </div>

    <button class="cart-button" @click="showCart()"> <img src=".\assets\cart.svg"> </button>


    <transition name="slide-fade">
      <div class="cart" v-if="isVisible">
        <h3>Items</h3>
        <div v-for="item in cart" :key="item[0]" class="cart-item-row">
          <p>{{ item[0] }} - {{ item[1] }}</p>
          <button
            @click="removeFromCart(item[0])"
            class="remove-item-button"
          >
            Remove
          </button>
        </div>
        <div class="button-container">
          <button id="checkout-button" @click="checkout()">Checkout</button>
        </div>
      </div>
    </transition>

    <!-- Items -->
    <div class="wrapper">
      <div class="card" style="width: 18rem;" v-for="item in items" :key="item">
        <img class="card-img-top" :src="item.item_src" alt="Card image cap">
        <div class="card-body">
          <h4 class="card-title"> {{ item.item_name }}</h4>
          <button @click="addToCart(item.item_name)"> Add to cart </button>
        </div>
      </div>
    </div>

  </div>
</template>

<style scoped>
.banner {
  background-color: #309898;
  padding: 2%;
}

.card-img-top {
  width: 100%;
  border-radius: 15px;
}

.button-container {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  gap: 5px;
}

#checkout-button {
  background-color: #FF9F00;
}

.cart {
  background-color: #237373;
  position: fixed;
  z-index: 1;
  top: 0;
  right: 0;
  width: 300px;
  height: 100%;
  padding: 20px;
  box-sizing: border-box;
  filter: drop-shadow(-5px 0px 15px #000000);
  transition: transform 0.3s ease-out;
}

.cart-button {
  z-index: 2;
  position: absolute;
  top: 0;
  right: 0;
  margin: 1%;
}

.cart-item-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 0;
  border-bottom: 1px solid #309898;
}

.remove-item-button {
  background-color: #ff6b6b;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9em;
}

.remove-item-button:hover {
  background-color: #ee5253;
}

.wrapper {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  justify-content: space-evenly;
}

.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(100%);
  opacity: 0;
}
</style>