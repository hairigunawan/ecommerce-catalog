<template>
  <div class="container" :class="pageClass">

    <div v-if="loading" class="card-loading">
      <div class="dots">
        <span></span>
        <span></span>
        <span></span>
      </div>
    </div>


    <div v-else-if="product" class="product-card">
      <div class="product-image">
        <img :src="product.image" :alt="product.title">
      </div>
      
      <div class="product-details">
        <div class="top-section">
          <h2 class="title">{{ product.title }}</h2>

          <div class="groub-category">
            <div class="category-row">
              <span class="category">{{ product.category }}</span>

              <div class="rating">
                <span class="rate">{{ product.rating.rate }}/5</span>

                <div 
                  class="circle"
                  v-for="n in 5"
                  :key="n"
                  :class="{ 'filled': n <= Math.round(product.rating.rate) }">
                </div>
              </div>

            </div>
          </div>

          <p class="description">{{ product.description }}</p>
        </div>

        <div class="bottom-section">
          <h3 class="price">${{ product.price }}</h3>
          <div class="btn-group">
            <button class="btn btn-buy">Buy now</button>
            <button class="btn btn-next" @click="nextProduct">Next product</button>
          </div>
        </div>
      </div>
    </div>

    <div v-else class="unavailable-card">
      <div class="unavailable-content">
        <p>This product is unavailable to show</p>
        <button class="btn btn-next-unavailable" @click="nextProduct">Next product</button>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      index: 1,
      product: null,
      loading: true,
      shownCount: 0,        
      isUnavailable: false  
    };
  },

  computed: {
    pageClass() {
      if (!this.product) return "";

      if (this.product.category === "men's clothing") return "page-men";
      if (this.product.category === "women's clothing") return "page-women";

      return "";
    }
  },

  methods: {
    async nextProduct() {

      if (this.isUnavailable) {
        this.shownCount = 0;
        this.isUnavailable = false;
        this.product = null;
        return this.fetchProduct();
      }

      this.index++;
      if (this.index > 20) this.index = 1;

      await this.fetchProduct();
    },

    async fetchProduct() {
      this.loading = true;

      if (this.shownCount >= 4) {
        this.product = null;
        this.isUnavailable = true;
        this.loading = false;
        return;
      }

      let valid = false;

      while (!valid) {
        const url = `https://fakestoreapi.com/products/${this.index}`;
        const response = await fetch(url);
        const data = await response.json();

        if (data.category === "men's clothing" || data.category === "women's clothing") {
          this.product = data;
          this.shownCount++;
          valid = true;
        } else {
          this.index++;
          if (this.index > 20) this.index = 1;
        }
      }

      this.loading = false;
    }
  },

  mounted() {
    this.fetchProduct();
  }
};
</script>


<style scoped>
@import '../assets/style/page.css';
</style>
