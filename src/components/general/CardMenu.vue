<template>
  <div class="row">
    <div class="col-xl-3 col-md-6 mb-4" v-for="menu in menus" :key="menu.nama_menu">
      <div class="card border-left-warning shadow h-100 py-2">
        <div class="card-body">
          <div class="row no-gutters align-items-center">
            <div class="col mr-2">
              <div class="text-xs font-weight-bold text-warning text-uppercase mb-1">
                {{ menu.nama_menu }}
              </div>
              <div class="col mb-2">
                <img :src="menu.image" height="100" alt="gambar" style="border-radius: 50%" />
              </div>
              <p>{{ menu.deskripsi_menu }}</p>
              <p>{{ menu.harga_menu }}</p>
              <div class="row">
                <!-- <div class="col-4">
                </div> -->
                  <button class="btn btn-info" @click="addToCart(menu)">Pilih</button>
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
  props: {
    menus: Array,
    addToCart: Function, // Tambahkan properti addToCart sebagai event untuk menambahkan item ke keranjang
  },
};
</script>

<style scoped>
#detail {
  margin-top: 15px;
}
</style>
