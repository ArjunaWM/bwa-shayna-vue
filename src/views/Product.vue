<template>
  <div class="product">
    <HeaderShayna />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more text-left">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img
                    class="product-big-img"
                    :src="default_product_picture"
                    alt=""
                  />
                </div>
                <div
                  class="product-thumbs"
                  v-if="productDetails.galleries.length > 0"
                >
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :items="3"
                    :nav="false"
                  >
                    <div
                      v-for="thumbs_image in productDetails.galleries"
                      :key="thumbs_image.id"
                      class="pt"
                      @click="changeImage(thumbs_image.image)"
                      :class="
                        thumbs_image.image == default_product_picture
                          ? 'active'
                          : ''
                      "
                    >
                      <img :src="thumbs_image.image" alt="" />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productDetails.type }}</span>
                    <h3>{{ productDetails.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p v-html="productDetails.description"></p>
                    <h4>$ {{ productDetails.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart">
                        <a href="#" @click="saveKeranjang(productDetails.id, productDetails.name, productDetails.price, productDetails.galleries[0].image)" class="primary-btn pd-cart">Add to cart</a>
                    </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedShayna />
    <FooterShayna />
  </div>
</template>

<script>
import HeaderShayna from "../components/HeaderShayna.vue";
import RelatedShayna from "../components/RelatedShayna.vue";
import FooterShayna from "../components/FooterShayna.vue";

import axios from "axios";

import carousel from "vue-owl-carousel";

export default {
  name: "product",
  components: {
    HeaderShayna,
    RelatedShayna,
    FooterShayna,
    carousel,
  },
  data() {
    return {
      default_product_picture: "",
      productDetails: [],
      keranjangUser: [],
    };
  },
  methods: {
    changeImage(urlImage) {
      this.default_product_picture = urlImage;
    },
    setDataImage(data) {
      this.productDetails = data;
      this.default_product_picture = data.galleries[0].image;
    },
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
    },
  },

  mounted() {
    if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }
    axios
      .get("http://127.0.0.1:8000/api/products", {
        params: {
          id: this.$route.params.id,
        },
      })
      .then((res) => this.setDataImage(res.data.data))
      .catch((err) => console.log(err));
  },
};
</script>

<style scoped>
.product-thumbs .pt {
  margin: 4px;
  /* margin-right: 6px; */
  /* margin-left: 6px; */
}
</style>
