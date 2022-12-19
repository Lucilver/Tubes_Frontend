<template>
  <v-main>
    <v-card class="black">
      <v-alert outlined color="orange">
        <v-list-item>
          <div>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title class="text-h4 pb-5 orange--text">Profil Saya</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <div class="photo">
              <img
                src="https://images.unsplash.com/photo-1511367461989-f85a21fda167?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8Mnx8cHJvZmlsZXxlbnwwfHwwfHw%3D&auto=format&fit=crop&w=500&q=60"
                style="border-radius: 50%; width: 400px; height: 300px"
              />
              <v-list-item-content class="ml-5 orange--text">
                <v-list-item-title class="headline mb-2 text-left"> <v-icon large class="orange mr-2">mdi-account-box</v-icon>Username : {{ users.username }} </v-list-item-title>
                <v-list-item-title class="headline mb-2 text-left"> <v-icon large color="orange mr-2">mdi-account</v-icon> Nama : {{ users.name }} </v-list-item-title>
                <v-list-item-title class="headline mb-2 text-left"> <v-icon large color="orange">mdi-email</v-icon> Email : {{ users.email }} </v-list-item-title>
              </v-list-item-content>
            </div>
          </div>
        </v-list-item>

        <v-card-title>
          <v-spacer></v-spacer>

          <v-btn color="light-blue darken-4" dark @click="(dialog = true), editHandler(users)"><v-icon>mdi-pencil-circle</v-icon></v-btn>

          <v-btn color=" ml-1 red darken-4" dark @click="deleteHandler(users.id)"><v-icon> mdi-delete-empty </v-icon></v-btn>
        </v-card-title>
      </v-alert>
    </v-card>

    <v-dialog v-model="dialog" persistent width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">{{ formTitle }} User</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-text-field v-model="form.name" label="Nama User" required></v-text-field>
            <v-text-field v-model="form.email" label="Email User" required></v-text-field>
            <v-text-field v-model="form.username" label="Email User" required></v-text-field>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="cancel"> Cancel </v-btn>
          <v-btn color="blue darken-1" text @click="setForm"> Save </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogConfirm" persistent max-width="400px">
      <div class="text-center">
        <v-sheet class="px-7 pt-7 pb-4 mx-auto text-center" dark>
          <div class="orange--text text--lighten-1 text-body-2 mb-4">Yakin ingin hapus User ini?</div>
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
      inputType: "Edit",
      load: false,
      snackbar: false,
      error_message: "",
      color: "",
      search: null,
      dialog: false,
      dialogConfirm: false,
      headers: [
        {
          text: "Nama User",
          align: "start",
          sortable: true,
          value: "name",
        },
        { text: "Email User", value: "email" },
        { text: "Username", value: "username" },
        { text: "Actions", value: "actions" },
      ],
      user: new FormData(),
      users: [],
      form: {
        name: null,
        email: null,
        username: null,
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
      var url = this.$api + "/getuser/" + localStorage.getItem("id");
      this.$http
        .get(url, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.users = response.data.data;
        });
    },

    update() {
      let newData = {
        name: this.form.name,
        email: this.form.email,
        username: this.form.username,
      };
      var url = this.$api + "/updateprofile/" + localStorage.getItem("id");
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
      var url = this.$api + "/user/" + this.deleteId;
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
          this.close();
          this.readData();
          this.inputType = "Tambah";
          this.$router.push({
            name: "Home",
          });
        })
        .catch((error) => {
          this.error_message = error.response.data.message;
          this.color = "red";
          this.snackbar = true;
          this.load = false;
        });
    },

    editHandler(users) {
      this.inputType = "Edit";
      this.editId = users.id;
      this.form.name = users.name;
      this.form.email = users.email;
      this.form.username = users.username;
      this.dialog = true;
    },

    deleteHandler(id) {
      this.deleteId = id;
      this.dialogConfirm = true;
    },

    close() {
      this.dialog = false;
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
        name: null,
        email: null,
        username: null,
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
  },
};
</script>

<style>
.photo {
  margin: auto;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  text-align: center;
}
.photo img {
  width: 100%;
  margin-left: 20%;
}
</style>
