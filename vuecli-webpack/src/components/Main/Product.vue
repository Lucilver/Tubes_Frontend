<template>
  <v-main class="list">
    <v-card dark>
      <v-alert outlined color="orange">
        <v-list-item>
          <v-list-item-avatar color="grey">
            <img src="https://st2.depositphotos.com/50337402/47106/v/600/depositphotos_471063438-stock-illustration-add-basket-gold-plated-metalic.jpg" />
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title class="headline">Produk</v-list-item-title>
            <v-list-item-subtitle>Kelompok J</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-card-title>
          <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" outlined hide details style="margin-top: 30px"></v-text-field>
          <v-spacer></v-spacer>
          <v-btn class="white--text" color="orange" dark @click="dialog = true"> Tambah </v-btn>
        </v-card-title>

        <v-card>
          <v-data-table :headers="headers" :items="products" :search="search">
            <template v-slot:[`item.actions`]="{ item }">
              <v-btn small class="mr-2 light-blue darken-4" @click="editHandler(item)"> <v-icon color="white">mdi-pencil</v-icon> </v-btn>
              <v-btn small class="mr-2 red darken-4" @click="deleteHandler(item.id)"> <v-icon color="white">mdi-delete</v-icon> </v-btn>
            </template>
          </v-data-table>
        </v-card>
      </v-alert>
    </v-card>
    <v-dialog v-model="dialog" persistent max-width="700px">
      <v-card class="grey lighten-3">
        <v-card-title>
          <span class="headline">{{ formTitle }} Jualan</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-text-field v-model="form.nama_barang" label="Nama Barang" required></v-text-field>
            <v-text-field prefix="$" v-model="form.harga_barang" label="Harga Barang" required></v-text-field>
            <v-text-field v-model="form.jumlah" label="Jumlah" required suffix="Barang"></v-text-field>
            <v-text-field v-model="form.warna" label="Warna" required></v-text-field>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red darken-1" text @click="cancel"> Cancel </v-btn>
          <v-btn color="green darken-1" text @click="setForm"> Save </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogConfirm" persistent max-width="400px">
      <div class="text-center">
        <v-sheet class="px-7 pt-7 pb-4 mx-auto text-center" dark>
          <div class="orange--text text--lighten-1 text-body-2 mb-4">Yakin ingin hapus produk ini?</div>

          <v-btn plain color="red" @click="dialogConfirm = false">No</v-btn>
          <v-btn plain color="green" @click="deleteData">Yes</v-btn>
        </v-sheet>
      </div>
    </v-dialog>

    <v-snackbar v-model="snackbar" :color="color" timeout="2000" bottom>
      {{ error_message }}
    </v-snackbar>
  </v-main>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      inputType: "Tambah",
      load: false,
      snackbar: false,
      error_message: "",
      color: "",
      search: null,
      idUser: 0,
      dialog: false,
      dialogConfirm: false,
      headers: [
        {
          text: "Nama Barang",
          align: "start",
          sortable: true,
          value: "nama_barang",
        },
        { text: "Harga Barang", value: "harga_barang" },
        { text: "Jumlah", value: "jumlah" },
        { text: "Warna", value: "warna" },
        { text: "Actions", value: "actions" },
      ],
      product: new FormData(),
      products: [],
      form: {
        nama_barang: null,
        harga_barang: null,
        jumlah: null,
        warna: null,
      },
      deleteId: "",
      editId: "",
    };
  },
  methods: {
    setForm() {
      if (this.inputType !== "Tambah") {
        this.update();
      } else {
        this.save();
      }
    },
    // Read Data Product
    readData() {
      var url = this.$api + "/product";
      this.$http
        .get(url, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.products = response.data.data;
        });
    },
// Tambah / jika tidak hosting
    save() {
      this.product.append("nama_barang", this.form.nama_barang);
      this.product.append("harga_barang", this.form.harga_barang);
      this.product.append("jumlah", this.form.jumlah);
      this.product.append("warna", this.form.warna);

      var url = this.$api + "/product";
      this.load = true;
      this.$http
        .post(url, this.product, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.error_message = response.data.message;
          this.color = "green";
          this.snackbar = true;
          this.load = true;
          this.close();
          this.readData();
          this.resetForm();
        })
        .catch((error) => {
          this.error_message = error.response.data.message;
          this.color = "red";
          this.snackbar = true;
          this.load = false;
        });
    },

    update() {
      let newData = {
        nama_barang: this.form.nama_barang,
        harga_barang: this.form.harga_barang,
        jumlah: this.form.jumlah,
        warna: this.form.warna,
      };
      var url = this.$api + "/product/" + this.editId;
      this.load = true;
      this.$http
        .put(url, newData, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.error_message = response.data.message;
          this.color = "green";
          this.snackbar = true;
          this.load = false;
          this.close();
          this.readData();
          this.resetForm();
          this.inputType = "Tambah";
        })
        .catch((error) => {
          this.error_message = error.response.data.message;
          this.color = "red";
          this.snackbar = true;
          this.load = false;
        });
    },

    deleteData() {
      // Menghapus Data
      var url = this.$api + "/product/" + this.deleteId;
      this.load = true;
      this.$http
        .delete(url, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.error_message = response.data.message;
          this.color = "green";
          this.snackbar = true;
          this.load = false;
          this.products.splice(this.index, 1);
          this.close();
          this.readData();
          this.resetForm();
          this.inputType = "Tambah";
        })
        .catch((error) => {
          this.error_message = error.response.data.message;
          this.color = "red";
          this.snackbar = true;
          this.load = false;
        });
    },

    editHandler(item) {
      this.inputType = "Edit";
      this.editId = item.id;
      this.form.nama_barang = item.nama_barang;
      this.form.harga_barang = item.harga_barang;
      this.form.jumlah = item.jumlah;
      this.form.warna = item.warna;
      this.dialog = true;
    },

    deleteHandler(id) {
      this.deleteId = id;
      this.dialogConfirm = true;
    },

    close() {
      this.dialog = false;
      this.inputType = "Tambah";
      this.dialogConfirm = false;
      this.readData();
    },

    cancel() {
      this.resetForm();
      this.readData();
      this.dialog = false;
      this.dialogConfirm = false;
      this.inputType = "Tambah";
    },

    resetForm() {
      this.form = {
        nama_barang: null,
        harga_barang: null,
        jumlah: null,
        warna: null,
      };
    },
  },

  computed: {
    formTitle() {
      return this.inputType;
    },
  },

  mounted() {
    this.readData();
    this.idUser = localStorage.getItem("id");
  },
};
</script>
