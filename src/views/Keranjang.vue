<template>
    <div class="keranjang">
        <AppNavbar :updateKeranjang="keranjangs" />
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
                            <li class="breadcrumb-item active" aria-current="page">Keranjang</li>
                        </ol>
                    </nav>
                </div>
            </div>

            <div class="row">
                <div class="col">
                    <h2>Keranjang <strong>Saya</strong></h2>
                    <div class="table-responsive mt-3">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th scope="col">#</th>
                                    <th scope="col">Foto</th>
                                    <th scope="col">Makanan</th>
                                    <th scope="col">Keterangan</th>
                                    <th scope="col">Jumlah</th>
                                    <th scope="col">Harga</th>
                                    <th scope="col">Total Harga</th>
                                    <th scope="col">Hapus</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(keranjang, index) in keranjangs" :key="keranjang.id">
                                    <th>{{ index + 1 }}</th>
                                    <td><img :src="'../assets/images/' + keranjang.products.gambar"
                                            class="img-fluid shadow" width="200"></td>
                                    <td><strong>{{ keranjang.products.nama }}</strong></td>
                                    <td>{{ keranjang.keterangan ? keranjang.keterangan : "-" }}</td>
                                    <td>{{ keranjang.jumlah_pemesanan }}</td>
                                    <td align="right">Rp. {{ keranjang.products.harga }}</td>
                                    <td align="right"><strong>Rp. {{ keranjang.products.harga *
                                        keranjang.jumlah_pemesanan
                                            }}</strong></td>
                                    <td align="center" class="text-danger">
                                        <b-icon-trash @click="hapusKeranjang(keranjang.id)"></b-icon-trash>
                                    </td>
                                </tr>
                                <tr>
                                    <td colspan="6" align="right">
                                        <strong>Total Harga : </strong>
                                    </td>
                                    <td align="right"><strong>Rp. {{ totalHarga }}</strong></td>
                                    <td></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Form Checkout -->
            <div class="row justify-content-end">
                <div class="col-md-4">
                    <form class="mt-4" v-on:submit.prevent>
                        <div class="form-group">
                            <label for="nama">Nama : </label>
                            <input type="text" class="form-control" v-model="pesan.nama" placeholder="Masukan Nama">
                        </div>
                        <div class="form-group">
                            <label for="noMeja">Nomor Meja : </label>
                            <input type="text" class="form-control" v-model="pesan.noMeja" placeholder="Masukan noMeja">
                        </div>
                        <button type="submit" class="btn btn-success mt-3 float-right" @click="checkout">
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
            keranjangs: [],
            pesan: {},
        }
    },
    methods: {
        setKeranjang(data) {
            this.keranjangs = data;
        },
        hapusKeranjang(id) {
            axios
                .delete(`http://localhost:3000/keranjangs/${id}`) // Menggunakan metode DELETE dan memperbaiki URL
                .then(() => {
                    this.$toast.success("Pesanan berhasil dihapus", { // Menggunakan toast.success untuk notifikasi sukses
                        position: 'top-right',
                        duration: 3000,
                        dismissible: true,
                    });
                    // Bisa menambahkan logika untuk memperbarui tampilan keranjang setelah penghapusan, misalnya:
                    this.keranjangs = this.keranjangs.filter(keranjang => keranjang.id !== id); // Update keranjang di frontend
                })
                .catch((error) => console.error(error));
        },
        checkout() {
            if (this.pesan.nama && this.pesan.noMeja) {
                this.pesan.keranjangs = this.keranjangs;
                axios
                    .post("http://localhost:3000/pesanans", this.pesan)
                    .then(() => {

                        this.keranjangs.map((item) => {
                            return axios
                                .delete(`http://localhost:3000/keranjangs/${item.id}`)
                                .then(() => {
                                    console.log(`Item dengan id: ${item.id} berhasil dihapus`);
                                })
                                .catch((error) => {
                                    console.error(`Error menghapus item dengan id: ${item.id}`, error);
                                });
                        });
                        this.$router.push({ path: "/pesanansukses" });
                        this.$toast.success("Berhasil Masuk Keranjang", {
                            position: 'top-right',
                            duration: 3000,
                            dismissible: true,
                        });
                    })
                    .catch(error => console.error(error));
            } else {
                this.$toast.error("Nama dan No Meja harus diisi", {
                    position: 'top-right',
                    duration: 3000,
                    dismissible: true,
                });
            }
        }
    },
    mounted() {
        axios
            .get("http://localhost:3000/keranjangs")
            .then((response) => {
                this.setKeranjang(response.data);
            })
            .catch((error) => console.error(error));
    },
    computed: {
        totalHarga() {
            return this.keranjangs.reduce(function (items, data) {
                return items + (data.products.harga * data.jumlah_pemesanan)
            }, 0)
        }
    }
}
</script>

<style></style>