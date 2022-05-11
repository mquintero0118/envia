<template>
  <v-container>
    <v-row no-gutters>
      <v-col md="auto">
        <v-card class="pa-2" elevation="0"> Numero de orden: </v-card>
      </v-col>
      <v-col lg="2">
        <v-card class="pa-2" elevation="0" v-if="selected"> {{selected[0].name}} </v-card>
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
    :headers="headers2"
    class="elevation-1"
      hide-default-foote
    >
    </v-data-table>
   

    <v-btn>logCOnsole</v-btn>
    <h2 v-if="selected">{{selected[0].items}}</h2>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "ordenesMain",
  data() {
    return {
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
  mounted() {
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
};
</script>
