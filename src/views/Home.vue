<template>
  <div class="container py-5">
    <div class="row">
      <div
        v-for="product in products"
        :key="product.id"
        class="col col-12 col-md-6 col-lg-3 mb-3"
      >
        <div class="card shadow rounded-3">
          <div class="card-img ratio ratio-4x3">
            <img class="img-fluid" :src="product.img" alt="Product image" />
          </div>
          <div class="card-body">
            <div>Име: {{ product.name }}</div>
            <div>Цена: {{ product.price }}денари</div>
            <div>
              Достапна количина:
              <span v-if="product.available_q !== 0">{{
                product.available_q
              }}</span>
              <span v-else> Нема </span>
            </div>
            <div class="mb-4">Нарачно: {{ product.order_q }}</div>
            <button @click="addToCart(product)" class="btn btn-primary">
              Додади во кошничка
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col col-12 col-md-6 col-lg-3 mx-auto">
        <CartItem :products="products" customer="Jordancho" />
      </div>
    </div>
  </div>
</template>

<script>
import CartItem from "../components/CartItem";
export default {
  name: "Home",
  components: { CartItem },
  methods: {
    addToCart(product) {
      if (!product.available_q) return;
      product.available_q--;
      product.order_q++;
      this.calculateTotalProducts();
    },
  },
  data() {
    return {
      products: [
        {
          id: 1,
          img: "https://upload.wikimedia.org/wikipedia/commons/a/ae/Carandache_Ecridor.jpg",
          name: "Пенкало",
          price: 1000,
          available_q: 100,
          order_q: 0,
        },
        {
          id: 2,
          img: "https://upload.wikimedia.org/wikipedia/en/thumb/0/09/Palomino_Blackwing_602_pencils.jpg/1920px-Palomino_Blackwing_602_pencils.jpg",
          name: "Молив",
          price: 150,
          available_q: 200,
          order_q: 0,
        },
        {
          id: 3,
          img: "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b6/Gutenberg_Bible%2C_Lenox_Copy%2C_New_York_Public_Library%2C_2009._Pic_01.jpg/894px-Gutenberg_Bible%2C_Lenox_Copy%2C_New_York_Public_Library%2C_2009._Pic_01.jpg",
          name: "Книга",
          price: 120,
          available_q: 20,
          order_q: 0,
        },
        {
          id: 4,
          img: "data:image/png;base64,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",
          name: "Блок",
          price: 400,
          available_q: 28,
          order_q: 0,
        },
        {
          id: 5,
          img: "https://4.imimg.com/data4/TN/NP/MY-32886991/a4-colour-paper-500x500.jpg",
          name: "Хартија во боја",
          price: 20,
          available_q: 23,
          order_q: 0,
        },
        {
          id: 6,
          img: "https://imgaz1.staticbg.com/thumb/large/oaupload/banggood/images/E2/89/becbcc05-7646-938c-07c9-3668a77f3de8.jpg",
          name: "Бои",
          price: 2344,
          available_q: 98,
          order_q: 0,
        },
        {
          id: 7,
          img: "data:image/jpeg;base64,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",
          name: "Телефон",
          price: 1000,
          available_q: 100,
          order_q: 0,
        },
        {
          id: 8,
          img: "https://www.ikea.com/kr/en/images/products/pokal-glass-turquoise__0713252_pe729362_s5.jpg",
          name: "Чаша",
          price: 25,
          available_q: 1100,
          order_q: 0,
        },
      ],
    };
  },
};
</script>
