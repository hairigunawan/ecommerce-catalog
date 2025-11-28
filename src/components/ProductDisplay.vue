<template>
  <div class="container" :class="pageClass">

    <div class="bg-pattern"></div>

    <div v-if="loading" class="card-loading">
      <div class="loader">Loading...</div>
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

    <div v-else class="product-card unavailable-card">
      <div class="unavailable-content">
        <p>This product is unavailable to show</p>
        <button class="btn btn-next-unavailable" @click="nextProduct">Next product</button>
      </div>
    </div>

  </div>
</template>

<script>

</script>


<style scoped>
@import '../assets/style/page.css';
</style>
