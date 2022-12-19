<template>
  <main>
    <v-container fluid fill-height class="position">
      <v-layout flex align-center justify-center>
        <v-card class="d-flex" flat tile>
          <v-img class="img" width="500" height="490" src="https://img.freepik.com/free-vector/mobile-login-concept-illustration_114360-83.jpg?size=338&ext=jpg&ga=GA1.2.628353381.1670783362&semt=sph"></v-img>
        </v-card>

        <v-card class="orange pa-5" width="400">
          <h1 class="text-center black orange--text pb-2">Register</h1>
          <v-card-text class="pt-4 orange">
            <div>
              <v-form v-model="valid" ref="form" class="white--text">
                <v-text-field label="Name" v-model="name" :rules="nameRules" required></v-text-field>
                <v-text-field label="Username" v-model="username" :rules="usernameRules" required></v-text-field>
                <v-text-field label="E-mail" v-model="email" :rules="emailRules" required></v-text-field>
                <v-text-field label="Password" v-model="password" type="password" min="8" :rules="passwordRules" counter required></v-text-field>

                <v-layout justify-center>
                  <v-btn class="mr-2" @click="submit" :class="{ 'orange darken-1 brown--text': valid, disabled: !valid }">Register</v-btn>
                  <v-btn @click="clear" class="grey darken-3 white--text">Clear</v-btn>
                </v-layout>
                <v-layout justify-center class="pt-3">
                  <v-text class="black--text"
                    >Have Account?
                    <v-btn @click="register" text color="indigo accent-4">Login</v-btn>
                  </v-text>
                </v-layout>
              </v-form>
            </div>
          </v-card-text>
        </v-card>
        <v-snackbar v-model="snackbar" :color="color" timeout="2000" bottom>{{ error_message }}</v-snackbar>
      </v-layout>
    </v-container>
  </main>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@500&display=swap");
.grey--text {
  font-family: "Roboto";
  text-align: end;
}

.position {
  position: absolute;
  left: 0;
  right: 0;
  background: linear-gradient(black, #ae7622);
}

.padding {
  top: 20px;
  text-align: center;
  background-color: black;
  color: orange;
}
</style>

<script>
export default {
  name: "UserRegister",
  data() {
    return {
      load: false,
      snackbar: false,
      error_message: "",
      color: "",
      valid: false,
      password: "",
      passwordRules: [(v) => !!v || "Password tidak boleh kosong"],
      email: "",
      emailRules: [(v) => !!v || "E-mail tidak boleh kosong"],
      name: "",
      nameRules: [(v) => !!v || "Nama Tidak boleh Kosong"],
      username: "",
      usernameRules: [(v) => !!v || "Username tidak boleh Kosong "],
    };
  },
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        // cek validasi data yang terkirim
        this.load = true;
        this.$http
          .post(this.$api + "/register", {
            name: this.name,
            username: this.username,
            email: this.email,
            password: this.password,
          })
          .then((response) => {
            localStorage.setItem("id", response.data.user.id);
            localStorage.setItem("token", response.data.access_token);
            this.error_message = response.data.message;
            this.color = "green";
            this.snackbar = true;
            this.load = false;
            this.clear();
            this.$router.push({
              name: "UserLogin",
            });
          })
          .catch((error) => {
            this.error_message = error.response.data.message;
            this.color = "red";
            this.snackbar = true;
            localStorage.removeItem("token");
            this.load = false;
          });
      }
    },
    register() {
      this.$router.push({
        name: "UserLogin",
      });
    },
    clear() {
      this.$refs.form.reset(); // clear form login
    },
  },
};
</script>
