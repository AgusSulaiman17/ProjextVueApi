<template>
  <div>
    <AppNavbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>Daftar Makanan</h2>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col">
          <div class="input-group mb-3">
            <input 
              v-model="search" 
              type="text" 
              class="form-control" 
              placeholder="Cari Makanan" 
              aria-label="Search" 
              aria-describedby="basic-addon1" 
              @input="searchFood"
            >
            <span class="input-group-text" id="basic-addon1">
              <b-icon-search></b-icon-search>
            </span>
          </div>
        </div>
      </div>

      <div class="row mb-4">
        <div class="col-12 col-sm-6 col-md-4 mb-3" v-for="product in products" :key="product.id">
          <Cardproduct :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppNavbar from '@/components/AppNavbar.vue';
import Cardproduct from '@/components/Cardproduct.vue';
import axios from 'axios';

export default {
  name: 'Foods',
  components: {
    AppNavbar,
    Cardproduct
  },
  data() {
    return {
      products: [],
      search: '',
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
    searchFood() {
      axios
        .get(`http://localhost:3000/products?q=${this.search}`)
        .then((response) => {
          this.setProducts(response.data);
        })
        .catch((error) => {
          console.error('Error fetching products:', error);
        });
    }
  },
  mounted() {
    axios
      .get("http://localhost:3000/products")
      .then((response) => {
        this.setProducts(response.data);
      })
      .catch((error) => console.error(error));
  },
};
</script>

<style scoped>
</style>
