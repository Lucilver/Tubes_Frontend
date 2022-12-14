<template>
    <v-main class="list">
        <v-card>
            <v-list-item>
                <v-list-item-avatar color="grey"> 
                    <img src="https://st2.depositphotos.com/50337402/47106/v/600/depositphotos_471063438-stock-illustration-add-basket-gold-plated-metalic.jpg">
                </v-list-item-avatar>
                <v-list-item-content>
                    <v-list-item-title class="headline">Wishlist</v-list-item-title>
                    <v-list-item-subtitle>Kelompok J</v-list-item-subtitle>
                </v-list-item-content>
            </v-list-item>
            <v-card-title>
                <v-text-field
                  v-model="search"
                  append-icon="mdi-magnify"
                  label="Search"
                  outlined
                  hide 
                  details 
                  style="margin-top: 30px"
                ></v-text-field>
                <v-spacer></v-spacer>
                <v-btn color="success" dark @click="dialog = true"> Tambah </v-btn>
            </v-card-title>
        </v-card>
        <v-card>
            <v-data-table :headers="headers" :items="todos" :search="search">
                <template v-slot:[`item.calories`]="{ item }">
                    <span v-if="(item.calories >= 400)">
                        <v-btn rounded color="red"> {{item.calories}} </v-btn>
                    </span>
                    <span v-else-if="(item.calories >= 300) && (item.calories < 400)">
                        <v-btn rounded color="yellow"> {{item.calories}}</v-btn>
                    </span>
                    <span v-else>
                        <v-btn rounded color="green"> {{item.calories}} </v-btn>
                    </span>
                </template>
                <template v-slot:[`item.actions`]="{ item }">
                    <v-btn icon color="blue"  @click="editItem(item)"> <v-icon>mdi-pencil</v-icon> </v-btn>
                    <v-btn icon color="red"  @click="deleteItem(item)"> <v-icon>mdi-delete</v-icon> </v-btn>
                </template>
            </v-data-table>
        </v-card>
        <v-dialog v-model="dialog" persistent max-width="600px">
        <v-card v-model="valid">
            <v-card-title>
                <span class="headline"> Makanan List </span>
            </v-card-title>
            <v-card-text>
                <v-container>
                    <v-text-field
                      v-model="listMakanan.name"
                      label="Name"
                      required 
                    ></v-text-field>
                    <v-text-field
                      v-model="listMakanan.calories"
                      label="Calories"
                      required 
                    ></v-text-field>

                    <v-textarea
                      v-model="listMakanan.note"
                      label="Note"
                      required 
                    ></v-textarea>
                    <v-select
                      v-model="listMakanan.status"
                      :items="[`On Process`, `Ready`, `Delivery`]"
                      label="Status"
                      required 
                    ></v-select>
                </v-container>
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="cancel"> Cancel </v-btn>
                <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
            </v-card-actions>
        </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
                <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
    </v-main>
</template>

<script>
export default {
    name: "ListItem",
    data() {
        return{
            search: null,
            dialog: false,
            headers: [
                {
                    text: "Nama Barang",
                    align: "start",
                    sortable: true,
                    value: "namaBarang",
                },
                { text: "Harga Barang", value: "hargaBarang" },
            ],
            editedIndex: -1,
            todos: [
                {
                    name: "Fried Rice",
                    calories: "120",
                    note: "Tidak Pake Cabe",
                    status: "On Process",
                },
                {
                    name: "Dessert",
                    calories: "300",
                    note: "Tambah Alat Makan",
                    status: "Ready",
                },
                {
                    name: "Ice Cream",
                    calories: "450",
                    note: "Rasa coklat dan stroberi",
                    status: "Delivery",
                },
            ],
            listMakanan: {
                name: null,
                calories: null,
                note: null,
                status: null,
            },
            defaultItem: {
                name: null,
                calories: null,
                note: null,
                status: null,
            },
        };
    },
    
    methods: {
        save() {
            if (this.editedIndex > -1) {
                Object.assign(this.todos[this.editedIndex], this.listMakanan)
            } else {
                this.todos.push(this.listMakanan)
            }
            this.resetForm();
            this.dialog = false;
        },
        cancel() {
            this.dialog = false
            this.$nextTick(() => {
                this.listMakanan = Object.assign({}, this.defaultItem)
                this.editedIndex = -1
            })
            this.resetForm();
        },
        editItem (item) {
            this.editedIndex = this.todos.indexOf(item)
            this.listMakanan = Object.assign({}, item)
            this.dialog = true
        },
        deleteItem (item) {
            this.editedIndex = this.todos.indexOf(item)
            this.listMakanan = Object.assign({}, item)
            this.dialogDelete = true
        },
        deleteItemConfirm () {
            this.todos.splice(this.editedIndex, 1)
            this.closeDelete()
        },
        closeDelete () {
            this.dialogDelete = false
            this.$nextTick(() => {
            this.listMakanan = Object.assign({}, this.defaultItem)
            this.editedIndex = -1
            })
        },
        resetForm() {
            this.listMakanan = {
                task: null,
                priority: null,
                note: null,
                status: null,
            };
        },
    },
};
</script>
<style>
  .text {
    font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
    font-size: 40px;
    font-style: italic;
  }
</style>