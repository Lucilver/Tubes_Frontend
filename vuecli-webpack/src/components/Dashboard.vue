<template>
  <v-main>
    <h1 class="text font-weight-medium mb-5 text-left">Dashboard</h1>

    <v-item-group>
      <v-container>
        <v-row>
          <v-col v-for="n in 3" :key="n" cols="12" md="4">
            <v-card :loading="loading" class="mx-auto my-12" max-width="374">
              <v-img height="250" src="https://gits.id/wp-content/uploads/2021/02/Contoh-Aplikasi-E-commerce-dan-Website.jpg"></v-img>

              <v-card-title><strong> E-commerce </strong></v-card-title>

              <v-card-text>
                <v-row align="center" class="mx-0">
                  <v-progress-linear value="60" height="15" striped color="deep-orange">
                    <template v-slot:default="{ value }">
                      <strong>{{ Math.ceil(value) }}%</strong>
                    </template></v-progress-linear
                  >
                </v-row>

                <div class="my-4 text-subtitle-1">
                  E-commerce (electronic commerce) is the buying and selling of goods and services, or the transmitting of funds or data, over an electronic network, primarily the internet. These business transactions occur either as
                  business-to-business (B2B), business-to-consumer (B2C), consumer-to-consumer or consumer-to-business.
                </div>
              </v-card-text>
              <v-divider class="mx-4"></v-divider>

              <v-card-title>Schedule</v-card-title>

              <v-card-text>
                <v-chip-group v-model="selection" active-class="deep-purple accent-4 white--text" column>
                  <v-chip>Sesi 1</v-chip>
                  <v-chip>Sesi 2</v-chip>
                  <v-chip>Sesi 3</v-chip>
                  <v-chip>Sesi 4</v-chip>
                </v-chip-group>
              </v-card-text>

              <v-card-actions>
                <v-btn color="deep-purple lighten-2" text @click="reserve"> Choose </v-btn>
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
    loading: false,
    selection: 1,
  }),
};
</script>
