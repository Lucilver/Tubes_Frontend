<template>
  <div class="dashboard">
    <v-navigation-drawer class="fullheight" width="256" v-model="drawer" absolute temporary color="white">
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="title">Kelompok J</v-list-item-title>
          <v-list-item-subtitle>WEB C</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" link color="light-blue darken-4" tag="router-link" :to="item.to">
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar color="red lighten-3" dark>
      <v-app-bar-nav-icon @click="drawer = true" color="white"></v-app-bar-nav-icon>
      <VSpacer />
    </v-app-bar>
    <v-app-bar app fixed height="75px" class="grey">
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <VSpacer />
      <v-toolbar-items>
        <v-btn text router @click="logout"><v-icon>mdi-power</v-icon></v-btn>
      </v-toolbar-items>
    </v-app-bar>

    <div class="fullheight pa-5">
      <router-view></router-view>
    </div>
  </div>
</template>
<script>
export default {
  name: "DasboardIndex",
  data() {
    return {
      drawer: false,
      group: null,
      items: [
        { title: "Dashboard", to: "/dashboard" },
        { title: "Product", to: "/product" },
        { title: "Wishlist", to: "/wishlist" },
        { title: "Toko", to: "/toko" },
        { title: "User", to: "/user" },
      ],
    };
  },
};
</script>
<style scoped>
.fullheight {
  min-height: 100vh !important;
}
</style>
