<template>
  <div id="wrapper">
    <Sidebar :class="sidebarClass" />
    <div id="content-wrapper" class="d-flex flex-column">
      <div id="content">
        <Navbar @toggle-sidebar="toggleSidebar" />
        <div class="container-fluid mt-4">
          <h1 class="h3 mb-0 text-gray-800 text-center">List Pesanan</h1>
          <div class="mt-4">
            <ul class="list-group">
              <li
                class="list-group-item d-flex justify-content-between align-items-center"
                v-for="(item, index) in cart"
                :key="index"
              >
                <span>{{ item.nama_menu }}</span>
                <span class="badge bg-primary rounded-pill">
                  {{ item.harga_menu }}
                </span>
                <span class="badge bg-secondary rounded-pill">
                  {{ item.quantity }}
                </span>
              </li>
            </ul>
            <div class="d-flex justify-content-between align-items-center mt-3">
              <h5>Total Bayar:</h5>
              <h5 class="text-primary">Rp.{{ total }}</h5>
            </div>
          </div>

          <!-- Tombol Bayar -->
          <button
            class="btn btn-primary mt-3"
            data-toggle="modal"
            data-target="#bayarModal"
          >
            Bayar
          </button>

          <!-- Pencarian menu -->
          <h1 class="h3 mb-0 text-gray-800 text-center mt-3 mb-5">List Menu</h1>
          <div class="input-group">
            <input
              type="text"
              v-model="searchTerm"
              class="form-control"
              placeholder="Cari Menu"
            />
            <button class="btn btn-outline-secondary" type="button">
              <i class="fas fa-search"></i>
            </button>
          </div>

          <!-- Tambahkan komponen CardMenu dan meneruskan data dan fungsi ke komponen anak -->
          <CardMenu :menus="filteredMenus" :addToCart="addToCart" />

          <!-- Modal Bayar -->
          <div
            class="modal fade"
            id="bayarModal"
            tabindex="-1"
            role="dialog"
            aria-labelledby="bayarModalLabel"
            aria-hidden="true"
          >
            <div class="modal-dialog" role="document">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title" id="bayarModalLabel">Pembayaran</h5>
                  <button
                    type="button"
                    class="close"
                    data-dismiss="modal"
                    aria-label="Close"
                  >
                    <span aria-hidden="true">&times;</span>
                  </button>
                </div>
                <div class="modal-body">
                  <p>Pilih metode pembayaran:</p>
                  <select class="form-control">
                    <option>Transfer Bank BCA</option>
                    <option>Transfer Bank Mandiri</option>
                    <option>Transfer Bank BRI</option>
                    <option>QRIS</option>
                  </select>
                </div>
                <div class="modal-footer">
                  <button
                    type="button"
                    class="btn btn-secondary"
                    data-dismiss="modal"
                  >
                    Tutup
                  </button>
                  <button type="button" class="btn btn-primary">Bayar</button>
                </div>
              </div>
            </div>
          </div>
          <!-- End Modal Bayar -->
        </div>
      </div>
      <Footer />
    </div>
  </div>
</template>

<script>
import Sidebar from "../components/admin/Sidebar.vue";
import Navbar from "../components/general/Navbar.vue";
import Footer from "../components/general/Footer.vue";
import CardMenu from "../components/general/CardMenu.vue";
import { ref, computed } from "vue";

export default {
  components: {
    Sidebar,
    Navbar,
    Footer,
    CardMenu,
  },
  setup() {
    const sidebarToggled = ref(false);
    const sidebarClass = ref("");
    const searchTerm = ref(""); // Data untuk menyimpan nilai pencarian
    const cart = ref([]); // Data untuk menyimpan item di keranjang

    const toggleSidebar = () => {
      sidebarToggled.value = !sidebarToggled.value;
      sidebarClass.value = sidebarToggled.value ? "toggle-sidebar" : "";
    };

    const dummyMenus = ref([
      {
        nama_menu: "Bebek Goreng",
        deskripsi_menu: "Bebek goreng dengan kematangan sempurna",
        harga_menu: "Rp.10000",
        image: "/img/menu1.jpg",
      },
      {
        nama_menu: "Bebek Bakar",
        deskripsi_menu: "Daging bebek yang dibakar, dipadukan dengan aroma bumbu khas.",
        harga_menu: "Rp.15000",
        image: "/img/menu2.jpg",
      },
      // Tambahkan data dummy lainnya jika diperlukan
    ]);

    // Filter menu berdasarkan kata kunci pencarian
    const filteredMenus = computed(() => {
      return dummyMenus.value.filter((menu) =>
        menu.nama_menu.toLowerCase().includes(searchTerm.value.toLowerCase())
      );
    });

    // Fungsi untuk menambahkan item ke keranjang
    const addToCart = (menu) => {
      // Cek apakah menu sudah ada di keranjang
      const existingItem = cart.value.find(
        (item) => item.nama_menu === menu.nama_menu
      );

      if (existingItem) {
        // Jika sudah ada, tingkatkan jumlahnya
        existingItem.quantity++;
      } else {
        // Jika belum ada, tambahkan sebagai item baru dengan jumlah 1
        cart.value.push({ ...menu, quantity: 1 });
      }
    };

    // Menghitung total harga di keranjang
    const total = computed(() => {
      return cart.value.reduce(
        (acc, item) =>
          acc + parseInt(item.harga_menu.replace("Rp.", "")) * item.quantity,
        0
      );
    });

    return {
      sidebarClass,
      toggleSidebar,
      dummyMenus,
      searchTerm,
      filteredMenus,
      addToCart,
      cart,
      total,
    };
  },
};
</script>

<style>
#content-wrapper {
  min-height: 780px !important;
}

/* Custom styles */
.input-group {
  max-width: 400px;
  margin-bottom: 20px;
}
</style>
