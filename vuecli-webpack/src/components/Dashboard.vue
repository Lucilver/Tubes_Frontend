<template>
  <v-main>
    <v-carousel height="90vh" dark cycle show-arrows-on-hover draggable="true" hide-delimiter-background class="text-center mb-5">
      <v-carousel-item v-for="(data, carindex) in data" :key="carindex" :src="data.src" class="gradient-fill">
        <v-container fill-height>
          <div style="max-width: 1200px">
            <div class="text-md-h2 mb-3 text-sm-h3 text-h5 font-weight-black orange--text text-center">
              {{ data.heading.toUpperCase() }}
            </div>
            <p class="mb-5 orange--text">{{ data.subHeading }}</p>
          </div>
        </v-container>
      </v-carousel-item>
    </v-carousel>
    <v-item-group>
      <v-container>
        <v-row>
          <v-col v-for="card in cards" :key="card.title" :cols="card.flex">
            <v-card outlined color="orange" prominent>
              <v-img height="250" :src="card.src" class="white--text align-end"> <v-card-title v-text="card.title" class="orange--text"></v-card-title></v-img>

              <v-card-text class="black orange--text">
                <v-card-title><strong> Toko Pojok</strong></v-card-title>
                <div class="my-4 text-subtitle-1">E-commerce (electronic commerce) is the buying and selling of goods and services, or the transmitting of funds or data</div>
              </v-card-text>
              <v-card-actions class="black">
                <v-btn color="black" class="orange ms-auto" @click="reserve">Beli </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-item-group>
  </v-main>
</template>
<style>
.text {
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
}

.gradient-fill .v-responsive__content {
  background: linear-gradient(to left, rgba(3, 12, 41, 0.8), rgba(5, 11, 31, 0.65));
}
.carousel-item img {
  height: 100vh !important;
}
</style>
<script>
export default {
  data: () => ({
    cards: [
      { title: "Buku", src: "https://media.istockphoto.com/id/532725717/photo/book-detail.jpg?s=170667a&w=0&k=20&c=YuuBllKczm6d5w6Ooizq_fDilhAN3aNAo2tbPXLLOGU=", flex: 3 },
      { title: "Hoodie", src: "https://images.unsplash.com/photo-1578768079052-aa76e52ff62e?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8MTJ8fGhvb2RpZXxlbnwwfHwwfHw%3D&auto=format&fit=crop&w=500&q=60", flex: 3 },
      { title: "Kursi", src: "https://images.unsplash.com/photo-1549497538-303791108f95?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8NHx8Y2hhaXJ8ZW58MHx8MHx8&auto=format&fit=crop&w=500&q=60", flex: 3 },
      { title: "Baju", src: "https://media.istockphoto.com/id/483425141/photo/blank-black-t-shirt-front-with-clipping-path.jpg?b=1&s=170667a&w=0&k=20&c=3ziJrkVuQp_tbC8PVzKKbCrSDS_5La0QsgFmlS0vcKE=", flex: 3 },
    ],
    data: [
      {
        src: "https://images.unsplash.com/photo-1523275335684-37898b6baf30?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8Mnx8cHJvZHVjdHxlbnwwfHwwfHw%3D&auto=format&fit=crop&w=500&q=60",
        heading: "Product",
        subHeading: "Quasha menyediakan produk - produk yang sudah terjamin kualitasnya dengan brand - brand yang sudah terkenal dan bisa dibayar dengan berbagai macam pembayaran.",
      },
      {
        src: "https://images.unsplash.com/photo-1617347454431-f49d7ff5c3b1?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8Mnx8ZGVsaXZlcnl8ZW58MHx8MHx8&auto=format&fit=crop&w=500&q=60",
        heading: "Delivery",
        subHeading: "Quasha menyediakan layanan pengiriman yang sangat cepat, aman, dan terpercaya serta gratis ongkir sampai dengan 50 ribu dengan syarat dan ketentuan yang berlaku.",
      },
      {
        src: "https://images.unsplash.com/photo-1580828343064-fde4fc206bc6?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8NHx8ZGlzY291bnR8ZW58MHx8MHx8&auto=format&fit=crop&w=500&q=60",
        heading: "Discount & Voucher",
        subHeading: "Quasha memberikan extra voucher sampai dengan 50 ribu kepada 100 orang yang beruntung dan diskon sampai dengan 50% pada event - event besar yang akan diselenggarakan.",
      },
    ],
  }),

  methods: {
    reserve() {
      this.$router.push({
        name: "Product",
      });
    },
  },
};
</script>
