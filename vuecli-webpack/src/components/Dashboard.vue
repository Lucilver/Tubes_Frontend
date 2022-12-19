<template>
  <v-main>
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
