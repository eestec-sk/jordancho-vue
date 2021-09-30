<template>
  <div class="card shadow">
    <div class="card-body">
      {{ customer }}
      <ul v-if="hasOrderedProducts">
        <li v-for="item in products" :key="item.id">
          <span v-if="item.order_q > 0"
            >{{ item.name }}, количина: {{ item.order_q }}, Вкупно чинење на
            продуктот: {{ calculateProductPrice(item) }}</span
          >
          <span v-else> {{ item.name }} Нема нарачано </span>
        </li>
      </ul>
      <div v-if="hasOrderedProducts">
        Вкупно производи: {{ calculateTotalProducts }}
      </div>
      <div v-if="hasOrderedProducts">
        Вкупна цена на производи {{ calculateTotalPrice }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CartItem",
  props: {
    products: {
      type: Array,
      required: true,
    },
    customer: {
      type: String,
      required: true,
    },
  },
  computed: {
    hasOrderedProducts() {
      for (let i = 0; i < this.products.length; i++) {
        if (this.products[i].order_q > 0) {
          return true;
        }
      }
      return false;
    },
    calculateProductPrice() {
      return (product) => product.order_q * product.price;
    },
    calculateTotalProducts() {
      let sum = 0;
      for (let i = 0; i < this.products.length; i++) {
        sum += this.products[i].order_q;
      }
      return sum;
    },
    calculateTotalPrice() {
      let sum = 0;
      for (let i = 0; i < this.products.length; i++) {
        sum += this.products[i].order_q * this.products[i].price;
      }
      return sum;
    },
  },
};
</script>

<style scoped></style>
