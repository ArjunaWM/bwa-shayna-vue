<template>
  <!-- Women Banner Section Begin -->
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12 mt-4" v-if="products.length > 0">
          <carousel
            class="product-slider"
            :items="3"
            :nav="true"
            :navText="['Left', 'Right']"
            :dots="false"
            :autoplay="true"
            :loop="true"
          >

            <div class="product-item" v-for="itemProduct in products" v-bind:key="itemProduct.id">
              <div class="pi-pic">
                <!-- <img src="img/mickey1.jpg" alt="" /> -->
                <img v-bind:src="itemProduct.galleries[0].image" alt="" />
                <ul>
                  <li @click="saveKeranjang(itemProduct.id, itemProduct.name, itemProduct.price, itemProduct.galleries[0].image)" class="w-icon active">
                    <a href="#"><i class="icon_bag_alt"></i></a>
                  </li>
                  <li class="quick-view">
                    <router-link v-bind:to="'/product/' + itemProduct.id">+ Quick View</router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ itemProduct.type }}</div>
                <a href="#">
                  <h5>{{ itemProduct.name }}</h5>
                </a>
                <div class="product-price">
                  ${{ itemProduct.price }}.00
                  <span>$35.00</span>
                </div>
              </div>
            </div>
            
          </carousel>
        </div>

        <div class="col-lg-12" v-else>
          <h1>Produk tidak tersedia</h1>
        </div>
        
      </div>
    </div>
  </section>
  <!-- Women Banner Section End -->
</template>

<script>
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  name: "WomanShayna",
  components: {
    carousel,
  },
  data() {
    return {
      products: [],
      keranjangUser: [],
    }
  },
  methods: {
    saveKeranjang(idProduct, nameProduct, priceProduct, imageProduct) {
      let productStored = {
        id: idProduct,
        name: nameProduct,
        price: priceProduct,
        image: imageProduct,
      };
      this.keranjangUser.push(productStored);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);

      window.location.reload();
    },
  },  
  mounted() {
    axios
      .get("http://127.0.0.1:8000/api/products")
      .then(res => (this.products = res.data.data.data))
      .catch(err => console.log(err));

      if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }
    
  }
};
</script>

<style scoped>
.product-item {
  margin-right: 25px;
}
.pi-pic img {
  height: 450px;
}
</style>