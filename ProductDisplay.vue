<template>
  <div class="main">
    <div v-if="isLoading" class="card">
      <div class="product-container">
        <div class="skeleton-photo"></div>
        <section class="skeleton-description">
          <div class="skeleton-title"></div>
          <div class="skeleton-details"></div>
        </section>
      </div>
    </div>
    <div v-else class="main" :class="!isProductAvailable || isLoading ? 'bg-gray' : isProductAvailable && product.data.category === 'men\'s clothing' ? 'bg-pattern bg-light-blue' : 'bg-pattern bg-pink'">
      <div class="image-container-background">
        <img src="../assets/img/bg-pattern.svg" alt="background pattern" />
      </div>
      <div class="card">
        <div v-if="!isProductAvailable" class="product-unavailable">
          <div class="image-container-background">
            <img src="../assets/img/sad-face.svg" alt="product unavailable" />
          </div>
          <div class="product-details">
            <p>This product is unavailable to show</p>
            <div class="button">
              <button type="button" @click="getProduct()" class="button-next">Next Product</button>
            </div>
          </div>
        </div>
        <div v-else class="product-container">
          <div class="product-thumbnail">
            <img :src="product.data.image" />
          </div>
          <div class="product-details">
            <div class="top">
              <h3 :class="product.data.category === 'men\'s clothing' ? 'font-dark-blue' : 'font-dark-purple'" class="title">{{ product.data.title }}</h3>
              <div class="sub-title">
                <span>{{ product.data.category }}</span>
                <div class="rating">
                  <span>{{ product.data.rating.rate }}/5</span>
                  <div class="rating">
                    <span v-for="n in 5" :key="n" :class="product.data.category === 'men\'s clothing' ? 'color-dark-blue' : 'color-dark-purple'" class="circle"></span>
                  </div>
                </div>
              </div>
              <div class="description">
                <p>{{ product.data.description }}</p>
              </div>
            </div>
            <div class="colors">
              <span :class="product.data.category === 'men\'s clothing' ? 'font-dark-blue' : 'font-dark-purple'" class="price">${{ product.data.price }}</span>
              <div class="button">
                <button type="button" :class="product.data.category === 'men\'s clothing' ? 'bg-dark-blue' : 'bg-dark-purple'" class="button-buy">Buy Now</button>
                <button type="button" @click="getProduct()" :class="product.data.category === 'men\'s clothing' ? 'border-dark-blue font-dark-blue' : 'border-dark-purple font-dark-purple'" class="button-next">Next Product</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      isLoading: false,
      index: 0,
      isProductAvailable: false,
      product: {},
    };
  },
  methods: {
    async callAPI() {
      const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
      const result = await response.json();
      return result;
    },

    async getProduct() {
      this.isLoading = true;

      if (this.index !== 20) {
        this.index++;
      } else {
        this.index = 1;
      }

      let data = await this.callAPI();
      if (data.category === "men's clothing" || data.category === "women's clothing") {
        this.product = { data };
        this.isProductAvailable = true;
      } else {
        this.isProductAvailable = false;
      }
      this.isLoading = false;
    },
  },
  mounted() {
    this.getProduct();
  },
};
</script>

<style scoped>
@import "../assets/style/page.css";
</style>
