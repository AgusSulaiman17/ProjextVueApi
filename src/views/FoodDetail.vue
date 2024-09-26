<template>
    <div class="food-detail">
        <AppNavbar />
        <div class="container">

            <!-- Breadcrumb -->
            <div class="row mt-5">
                <div class="col">
                    <nav aria-label="breadcrumb">
                        <ol class="breadcrumb">
                            <li class="breadcrumb-item">
                                <router-link to="/" class="text-dark">Home</router-link>
                            </li>
                            <li class="breadcrumb-item">
                                <router-link to="/foods" class="text-dark">Foods</router-link>
                            </li>
                            <li class="breadcrumb-item active" aria-current="page">Food order</li>
                        </ol>
                    </nav>
                </div>
            </div>

            <div class="row">
                <div class="col-md-6">
                    <!-- Menampilkan gambar produk -->
                    <img :src="'../assets/images/' + product.gambar" class="img-fluid shadow" alt="...">
                </div>
                <div class="col-md-6">
                    <!-- Menampilkan nama dan harga produk -->
                    <h2><strong>{{ product.nama }}</strong></h2>
                    <hr>
                    <h4>Harga :<strong> Rp. {{ product.harga }}</strong></h4>

                    <!-- Form pemesanan -->
                    <form class="mt-4" v-on:submit.prevent="pemesanan">
                        <div class="form-group">
                            <label for="jumlah_pemesanan">Jumlah pesan</label>
                            <input type="number" class="form-control" v-model="pesan.jumlah_pemesanan"
                                placeholder="Masukkan jumlah pesanan">
                        </div>
                        <div class="form-group">
                            <label for="keterangan">Keterangan</label>
                            <textarea v-model="pesan.keterangan" class="form-control"
                                placeholder="Keterangan spt: Pedas, Nasi Setengah, dll."></textarea>
                        </div>
                        <button type="submit" class="btn btn-success mt-3">
                            <b-icon-cart></b-icon-cart> Pesan
                        </button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import AppNavbar from '@/components/AppNavbar.vue';
import axios from 'axios';

export default {
    name: 'FoodDetail',
    components: {
        AppNavbar
    },
    data() {
        return {
            product: {},
            pesan: {
            }
        }
    },
    methods: {
        setProduct(data) {
            this.product = data;
        },
        pemesanan() {
            if (this.pesan.jumlah_pemesanan) {
                this.pesan.products = this.product;
                axios
                    .post("http://localhost:3000/keranjangs", this.pesan)
                    .then(() => {
                        this.$router.push({ path: "/keranjang"})
                        this.$toast.success("Berhasil Masuk Keranjang", {
                            type: 'success',
                            position: 'top-right',
                            duration: 3000,
                            dismissible: true,
                        })
                    })
                    .catch((err) => console.log(err));
            } else {
                this.$toast.error("Jumlah pesanan Harus di isi", {
                    type: 'success',
                    position: 'top-right',
                    duration: 3000,
                    dismissible: true,
                })
            }
        }
    },
    mounted() {
        axios
            .get("http://localhost:3000/products/" + this.$route.params.id)
            .then((response) => {
                this.setProduct(response.data);
            })
            .catch((error) => console.error(error));
    }
}
</script>

<style></style>
