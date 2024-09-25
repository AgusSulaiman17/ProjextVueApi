<template>
  <div class="home">
    <AppNavbar></AppNavbar>
    <div class="container">
      <Hero></Hero>
      <div class="row mt-4 mb-4">
        <div class="col">
          <h2>Makanan <strong>Terbaik</strong></h2>
        </div>
        <div class="col">
          <router-link to="/foods" class="btn btn-success float-right"><b-icon-eye></b-icon-eye> Lihat
            Semua</router-link>
        </div>
      </div>

      <div class="row mb-3">
        <div class="col-12 col-sm-6 col-md-4 mb-3" v-for="product in products" :key="product.id">
          <Cardproduct :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppNavbar from '@/components/AppNavbar.vue';
import Hero from '@/components/Hero.vue';
import Cardproduct from '@/components/Cardproduct.vue';
import axios from 'axios';
// @ is an alias to /src
export default {
  name: 'HomeView',
  components: {
    AppNavbar,
    Hero,
    Cardproduct
  },
  data() {
    return {
      products: []
    }
  },
  methods: {
    setProducts(data) {
      this.products = data
    }
  },
  mounted() {
    axios
      .get("http://localhost:3000/best-products")
      .then((response) => this.setProducts(response.data))
      .catch((eror) => console.log(eror))
  },
};
</script>
