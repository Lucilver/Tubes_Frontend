<template>
  <div class="dashboard">
    <v-navigation-drawer class="fullheight" width="256" v-model="drawer" absolute temporary color="black darken-5">
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h5 bold orange--text">Kelompok J</v-list-item-title>
          <v-list-item-subtitle class="text-h6 orange--text">WEB C</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider class="orange"></v-divider>

      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" link color="light-blue darken-4" tag="router-link" :to="item.to">
          <v-list-item-content>
            <v-list-item class="text-h6 orange--text">
              <v-icon class="mr-2" color="orange">{{ item.icon }}</v-icon
              >{{ item.title }}
            </v-list-item>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar color="black">
      <v-app-bar-nav-icon @click="drawer = true" color="orange"></v-app-bar-nav-icon>
      <VSpacer />
      <v-toolbar-items>
        <v-btn text router @click="logout()" color="orange"><v-icon>mdi-power</v-icon></v-btn>
      </v-toolbar-items>
    </v-app-bar>
    <div class="fullheight pa-5 black">
      <router-view></router-view>
    </div>
  </div>
</template>
<script>
export default {
  name: "DashboardLayout",
  data() {
    return {
      users: [],
      drawer: false,
      items: [
        { title: "Dashboard", to: "/dashboard", icon: "mdi-view-dashboard" },
        { title: "Toko", to: "/toko", icon: "mdi-store" },
        { title: "Produk", to: "/product", icon: "mdi-shopping" },
        { title: "Pengiriman", to: "/delivery", icon: "mdi-truck-delivery" },
        { title: "User", to: "/user", icon: "mdi-shield-account" },
      ],
    };
  },
  methods: {
    logout() {
      localStorage.removeItem("id");
      localStorage.removeItem("token");
      location.reload();
      this.$router.push({
        name: "Home",
      });
    },
    readData() {
      var url = this.$api + "/getuser/" + localStorage.getItem("id");
      this.$http
        .get(url, {
          headers: {
            Authorization: "Bearer" + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.users = response.data.data;
        });
    },
  },

  mounted() {
    this.readData();
  },
};
</script>
<style scoped>
.fullheight {
  min-height: 100vh !important;
}
.router {
  text-decoration: none;
  color: black;
}
</style>
