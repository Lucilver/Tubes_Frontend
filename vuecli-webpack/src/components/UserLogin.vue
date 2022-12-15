<template>
  <main>
    <v-container fluid fill-height class="view">
      <v-layout flex align-center justify-center>
        <v-card class="d-flex" flat tile>
          <v-img
            class="img"
            width="500"
            height="450"
            src="https://img.freepik.com/free-vector/computer-login-concept-illustration_114360-7962.jpg?w=740&t=st=1670783380~exp=1670783980~hmac=07e14766942c2c636a3e0ac9236f84d729997de462ec7bad0029aa682bf8591f"
          ></v-img>
        </v-card>

        <v-card class="orange pa-5" width="500" height="450">
          <h1 class="text-center black orange--text pb-2">Login</h1>
          <v-card-text class="pt-2">
            <div>
              <v-form v-model="valid" ref="form" class="mt-5">
                <v-text-field label="E-mail" v-model="email" :rules="emailRules" required></v-text-field>
                <v-text-field label="Password" v-model="password" type="password" min="8" :rules="passwordRules" counter required></v-text-field>
                <v-layout justify-center>
                  <v-btn class="mr-3 mt-5" @click="submit" :class="{ 'black darken-1 orange--text': valid, disabled: !valid }">Login</v-btn>
                  <v-btn @click="clear" class="grey darken-3 white--text mt-5">Clear</v-btn>
                </v-layout>
                <v-layout justify-center class="pt-3 mt-5">
                  <v-text>Dont have any account? <v-btn @click="register" text color="indigo accent-4">Register</v-btn></v-text>
                </v-layout>
              </v-form>
            </div>
          </v-card-text>
        </v-card>
      </v-layout>
    </v-container>
  </main>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@500&display=swap");
.grey--text {
  font-family: "Roboto";
}

.view {
  position: absolute;
  left: 0;
  right: 0;
  background: linear-gradient(black, #ae7622);
}

.img {
  background-color: black;
}
</style>

<script>
export default {
  name: "UserLogin",
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
      emailRules: [(v) => !!v || "E-mail tidak boleh kosong "],
    };
  },
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        // cek validasi data yang terkirim
        this.load = true;
        this.$http
          .post(this.$api + "/login", {
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
              name: "Dashboard",
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
        name: "UserRegister",
      });
    },
    clear() {
      this.$refs.form.reset(); // clear form login
    },
  },
};
</script>
