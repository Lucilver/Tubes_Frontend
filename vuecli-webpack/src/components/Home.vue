<template>
  <main>
    <v-header dark padless>
      <v-card flat tile class="black lighten-1 white--text d-flex pb-3">
        <v-card class="pa-2 mr-auto black mt-3 ml-5">
          <v-text class="orange--text"> <strong>Kelompok J</strong> </v-text></v-card
        >
        <v-card class="pa-2 black mt-3">
          <v-btn @click="login" class="grey darken-4 large orange--text text-right">Login</v-btn>
        </v-card>
        <v-card class="pa-2 black mt-3 mr-5">
          <v-btn @click="register" class="orange black--text text-right">Register</v-btn>
        </v-card>
      </v-card>
    </v-header>
    <v-carousel height="90vh" dark cycle show-arrows-on-hover draggable="true" hide-delimiter-background class="text-center">
      <v-carousel-item v-for="(data, carindex) in data" :key="carindex" :src="data.src" class="gradient-fill">
        <v-container fill-height>
          <div style="max-width: 1200px">
            <div class="text-md-h2 mb-3 text-sm-h3 text-h5 font-weight-black orange--text">
              {{ data.heading.toUpperCase() }}
            </div>
            <p class="mb-5 orange--text">{{ data.subHeading }}</p>
          </div>
        </v-container>
      </v-carousel-item>
    </v-carousel>
    <v-footer dark padless>
      <v-card flat tile class="flex">
        <v-card-title class="black orange--text text-center">
          <strong class="subheading">Get connected with us on social networks!</strong>

          <v-spacer></v-spacer>
          <v-btn color="orange" icon> <v-icon> mdi-facebook </v-icon> </v-btn>
          <v-btn color="orange" icon> <v-icon> mdi-instagram </v-icon> </v-btn>
          <v-btn color="orange" icon> <v-icon> mdi-twitter </v-icon> </v-btn>
          <v-btn color="orange" icon> <v-icon> mdi-linkedin </v-icon> </v-btn>
        </v-card-title>

        <v-divider></v-divider>

        <v-card-text class="black orange--text text-center"> {{ new Date().getFullYear() }} — <strong>Created by Alvin Cennanda | Frederik Grayfien Halim | Bernadeta Felicia Astrid | Satria Bagus Yudatama</strong> </v-card-text>
      </v-card>
    </v-footer>
  </main>
</template>

<script>
export default {
  data() {
    return {
      data: [
        {
          src: "https://img.freepik.com/free-vector/happy-people-shopping-online_74855-5865.jpg?w=2000",
          heading: "Quasha",
          subHeading:
            "Infographic hypotheses influencer user experience Long madel ture gen-z paradigm shift client partner network product seilans solve management influencer analytics leverage virality. incubator seed round massmarket. buyer agile development growth hacking business-to-consumer ecosystem ",
        },
        {
          src: "https://freeillustrations.xyz/wp-content/uploads/2020/12/E-Commerce-Illustration-Kit@4x.png",
          heading: "Quasha",
          subHeading: "Litora a interdum primis. Lectus facilisi sit cum arcu aliquam velit imperdiet sapien suspendisse at vel facilisis. Facilisis metus, curae; nibh mi orci et. Fringilla sociis magna rutrum bibendum arcu.",
        },
        {
          src: "https://img.freepik.com/free-vector/ecommerce-web-page-concept-illustration_114360-8204.jpg?w=2000",
          heading: "Quasha",
          subHeading:
            "Tellus cum turpis natoque id justo interdum tincidunt faucibus aptent congue pellentesque. Proin luctus orci netus! Scelerisque parturient cursus donec parturient et, luctus aptent habitant aliquet felis. Sapien placerat ultricies.",
        },
      ],
    };
  },
  methods: {
    register() {
      this.$router.push({
        name: "UserRegister",
      });
    },
    login() {
      this.$router.push({
        name: "UserLogin",
      });
    },
  },
};
</script>

<style>
.gradient-fill .v-responsive__content {
  background: linear-gradient(to left, rgba(3, 12, 41, 0.8), rgba(5, 11, 31, 0.65));
}
.carousel-item img {
  height: 100vh !important;
}
</style>
