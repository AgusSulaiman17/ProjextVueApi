<template>
  <div class="">
    <AppNavbar />
    <div class="container">
        <div class="row justify-content-center mt-5">
            <div class="col text-center">
                <img src="../assets/images/menunggu.png" alt="" width="300">
                <h2><strong>Sukses !!!</strong></h2>
                <h4>Pesanan Anda Segera di proses <br> Selamat menunggu</h4>
                 <router-link to="/" class="btn btn-success mt-3">Kembali ke Home</router-link>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import AppNavbar from '@/components/AppNavbar.vue';

export default {
    name: 'PesananSukses',
    components: {
        AppNavbar
    },
}
</script>

<style>

</style>