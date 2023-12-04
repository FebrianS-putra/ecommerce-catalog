<template>
  <div class="container">
    <div v-if="loading" class="loading-page">
      <!-- <div class="loader"></div> -->
      <div class="lds-ellipsis"><div></div><div></div><div></div><div></div></div>
    </div>
    <!-- ok0 -->
    <!-- start crd container -->
    <div v-else class="container" 
    :class="!productAvailable? 'theme-default' : product.data.category === 
    'men\'s clothing' ? 'theme-blue' : 'theme-pink'">
    <div class="overlay"><img src="../assets/shape.svg" alt="background shape"></div>
    <div class="card">
      <!-- Unavailable crd section strat -->
      <div v-if="!productAvailable" class="items-unavailable">
        <div class="overlay"><img src="../assets/sad-face.svg" alt="unavailable item background sad"></div>
        <div class="product-details">
          <p>This product is unavailable to show</p>
          <div class="btn_container"><button type="button" @click="getOneProduct()" class="btn_next">Next Product</button></div>
        </div>
      </div>
      <!-- Unavailable crd section end -->
      <div v-else class="item-container">
        <div class="item-picture"><img :src="product.data.image" alt="picture item"></div>
        <div class="product-details">
          <div class="top">
            <h3 :class="product.data.category === 'men\'s clothing' ? 'font-navy' : 'font-magenta'" class="title"> {{ product.data.title }} </h3>
            <div class="category_ranting">
              <span> {{ product.data.category }} </span>
              <div class="rating">
                <p> {{ product.data.rating.rate }}/5 </p>
                <div class="rating">
                  <p 
                  v-for="index in 5 "
                  :key="index" class="circle"
                  :class=" product.data.category === 'men\'s clothing' ? 'theme-navy' : 'theme-magenta' "
                   ></p>
                </div>
              </div>
            </div>
            <div class="description_product"><p>{{ product.data.description }}</p></div>
          </div>
          <!-- button and price -->
          <div class="p_price">
            <span :class="product.data.category === 'men\'s clothing' ? 'font-navy' : 'font-magenta'" class="price">${{ product.data.price }}</span>
          </div>
          <div class="btn_container">
            <button type="button"  :class="product.data.category === 'men\'s clothing' ? 'theme-navy' : 'theme-magenta'" class="btn_buy">Buy Now</button>
            <button type="button" @click="getOneProduct()" :class="product.data.category === 'men\'s clothing' ? 'border-navy font-navy' : 'border-magenta font-magenta'" class="btn_next">Next Product</button>
          </div>
          <!-- button and price end -->
        </div> 
      </div>
    </div>
    </div>
    <!-- end crd container  -->
  </div>
</template>

<script>
export default {
  name: 'DisplayCatalog',
  data() {
    return {
      product: {},
      loading: false,
      index: 0,
      productAvailable: false,
    }
  },
  methods: {
    async wgetAPI() {
      const response = await fetch(`https://fakestoreapi.com/products/${this.index}`)
      const result = await response.json()
      return result;
      // console.log(result);
    },
    async getOneProduct() {
      this.loading = true;
      if (this.index !== 20) {
        this.index++
      } else {
        this.index = 1;
      }
      let data = await this.wgetAPI()
      if (data.category === "men's clothing" || data.category === "women's clothing") {
        this.product = { data }
        this.productAvailable = true;
      } else {
        this.productAvailable = false;
      }
      this.loading = false;
    }
  }, mounted() {
    this.getOneProduct();
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import '../assets/page-style.css';

/* animation loader*/
.loading-page {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
.lds-ellipsis {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-ellipsis div {
  position: absolute;
  top: 33px;
  width: 13px;
  height: 13px;
  border-radius: 50%;
  background: #dcdcdc;
  animation-timing-function: cubic-bezier(0, 1, 1, 0);
}
.lds-ellipsis div:nth-child(1) {
  left: 8px;
  animation: lds-ellipsis1 0.6s infinite;
}
.lds-ellipsis div:nth-child(2) {
  left: 8px;
  animation: lds-ellipsis2 0.6s infinite;
}
.lds-ellipsis div:nth-child(3) {
  left: 32px;
  animation: lds-ellipsis2 0.6s infinite;
}
.lds-ellipsis div:nth-child(4) {
  left: 56px;
  animation: lds-ellipsis3 0.6s infinite;
}
@keyframes lds-ellipsis1 {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes lds-ellipsis3 {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(0);
  }
}
@keyframes lds-ellipsis2 {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(24px, 0);
  }
}


</style>
