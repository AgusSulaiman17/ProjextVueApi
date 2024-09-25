<template>
    <div class="hero">

        <!-- Dekstop -->
        <div class="d-none d-md-block">
            <div class="row mt-4">
                <div class="col-md-6">
                    <div class="d-flex h-100">
                        <div class="justify-content-center align-self-center">
                            <h2><strong>Makanan Yang Menarik </strong><br>Di Handphonemu</h2>
                            <p>Ayo segera pilih makan yang anda inginkan</p>
                            <button class="btn btn-lg btn-success"><b-icon-arrow-right></b-icon-arrow-right>
                                Pesan</button>
                        </div>
                    </div>
                </div>
                <div class="col d-flex justify-content-end">
                    <img src="../assets/images/hero.png" alt="" class="img-fluid w-100 w-sm-50 w-md-25 w-lg-10">
                </div>
            </div>
        </div>

        <!-- Mobile -->
        <div class="d-sm-block d-md-none">
            <div class="row mt-4">
                <div class="col">
                    <img src="../assets/images/hero.png" alt="" class="img-fluid w-100 w-sm-50 w-md-25 w-lg-10">
                </div>
                <div class="col-md-6 mt-4">
                    <div class="d-flex h-100">
                        <div class="justify-content-center align-self-center">
                            <h2><strong>Makanan Yang Menarik </strong><br>Di Handphonemu</h2>
                            <p>Ayo segera pilih makan yang anda inginkan</p>
                            <router-link to="/foods" class="btn btn-success float-right"><b-icon-eye></b-icon-eye> Lihat
                                Semua</router-link>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Hero'
}
</script>

<style scoped>
.heroimg {
    width: 500px;
}
</style>