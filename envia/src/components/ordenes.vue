<template>
  <v-container>
    <v-row no-gutters>
      <v-col md="auto">
        <v-card class="pa-2" elevation="0"> Numero de orden: </v-card>
      </v-col>
      <v-col lg="2">
        <v-card class="pa-2" elevation="0" v-if="selected">
          {{ selected[0].name }}
        </v-card>
      </v-col>
    </v-row>
    <v-data-table
      v-model="selected"
      :headers="headers"
      :items="items"
      :single-select="singleSelect"
      item-key="id"
      show-select
      class="elevation-1"
      hide-default-footer
    >
    </v-data-table>
    <v-divider class="mt-4 mb-4"></v-divider>
    <v-data-table
      class="elevation-1"
      :headers="header2"
      hide-default-footer
      :items="itemsDetail"
      v-if="itemsDetail"
    >
      <template v-slot:footer>
        <v-divider class="mb-2"></v-divider>
        <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              class="mx-2 mb-2"
              fab
              dark
              color="indigo"
              @click="dialog = !dialog"
              v-bind="attrs"
              v-on="on"
            >
              <v-icon dark> mdi-plus </v-icon>
            </v-btn>
          </template>
          <span>Agregar producto</span>
        </v-tooltip>
      </template>
    </v-data-table>
    <v-dialog v-model="dialog" width="35%">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          Agregar producto
        </v-card-title>
        <v-card-text class="mt-5">
          <v-row>
            <v-col cols="6"> <v-text-field outlined dense label="Sku"></v-text-field></v-col>
            <v-col cols="6"><v-text-field outlined dense label="Nombre"></v-text-field></v-col>
          </v-row>
         <v-row>
            <v-col cols="6"> <v-text-field outlined dense label="Cantidad"></v-text-field></v-col>
            <v-col cols="6"><v-text-field outlined dense label="Precio"></v-text-field></v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="addItem"> Guardar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- <v-btn>logCOnsole</v-btn> -->
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "ordenesMain",
  data() {
    return {
      dialog: false,
      singleSelect: true,
      selected: null,
      itemsDetail: [],
      items: [],
      header2: [
        {
          text: "Sku",
          align: "start",
          sortable: false,
          value: "sku",
        },
        {
          text: "Nombre",
          align: "start",
          sortable: false,
          value: "name",
        },
        {
          text: "Cantidad",
          align: "start",
          sortable: false,
          value: "quantity",
        },
        {
          text: "Precio",
          align: "start",
          sortable: false,
          value: "price",
        },
      ],
      headers: [
        {
          text: "Numero de orden",
          align: "start",
          sortable: false,
          value: "id",
        },
        {
          text: "Nombre de la orden",
          sortable: false,
          value: "name",
          align: "start",
        },
      ],
    };
  },
  watch: {
    selected() {
      this.itemsDetail = [];
      console.log(this.selected);
      this.selected[0].items.forEach((element) => {
        this.itemsDetail.push(element);
      });
      console.log(this.itemsDetail);
    },
  },
  mounted() {
    this.getProducts();
  },
  methods: {
    addItem() {
      this.dialog = !this.dialog;
    },
    getProducts() {
      const header1 =
        "eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJwUGFINU55VXRxTUkzMDZtajdZVHdHV3JIZE81cWxmaCIsImlhdCI6MTYyMDY2Mjk4NjIwM30.lhfzSXW9_TC67SdDKyDbMOYiYsKuSk6bG6XDE1wz2OL4Tq0Og9NbLMhb0LUtmrgzfWiTrqAFfnPldd8QzWvgVQ";
      axios
        .get("https://eshop-deve.herokuapp.com/api/v2/orders", {
          headers: {
            Authorization: header1,
          },
        })
        .then((res) => {
          console.log(res.data);
          res.data.orders.forEach((element) => {
            this.items.push(element);
          });
          console.log(this.items);
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>
