<template>
  <div id="app">
    <product-card
      :items="items"
      :options="options"
      @widgetClick="add_to_cart"
      @widgetRating="setRating"
      @widgetFavorite="favoriteItem"
    />
  </div>
</template>

<script>
import ProductCard from "./components/WidgetBox.Product.Card.vue";

export default {
  name: "app",
  data: () => ({
    items: [
      {
        id: "1",
        name: "Margherita",
        price: 22,
        sale_price: 0,
        label: "Sliced Pizza",
        image:
          "https://storage.googleapis.com/ghostfood-539ae.appspot.com/images/pizza_2.jpg",
        isfavorite: false
      },
      {
        id: "2",
        name: "Capricciosa",
        price: 25,
        sale_price: 22,
        label: "Sliced Pizza",
        image:
          "https://storage.googleapis.com/ghostfood-539ae.appspot.com/images/pizza_3.jpg",
        isfavorite: false,
        rating: 3.5
      },
      {
        id: "3",
        name: "Napoli",
        price: 20,
        sale_price: 0,
        label: "Sliced Pizza",
        image:
          "https://storage.googleapis.com/ghostfood-539ae.appspot.com/images/pizza_4.jpg",
        isfavorite: false
      },
      {
        id: "3",
        name: "Salame Piccante",
        price: 19,
        sale_price: 0,
        label: "Sliced Pizza",
        image:
          "https://storage.googleapis.com/ghostfood-539ae.appspot.com/images/pizza_5.jpg",
        isfavorite: false
      }
    ],
    options: {
      color: "#fff",
      title_color: "#555",
      text_color: "#ccc",
      price_color: "#555",
      sale_color: "#ff0000",
      icons_color: "#888",
      rating_color: "#ff0000",
      elevation: "1",
      rating: true,
      favorite: true,
      currency: "€",
      salebox: true
    },
    product: {
      id: 1,
      name: "T-Shirt"
    }
  }),
  components: {
    ProductCard
  },
  methods: {
    add_to_cart(event) {
      console.log("add_to_cart product=>", event);
    },
    setRating(event, product) {
      console.log("rating=>", event, "product=>", product);
    },
    favoriteItem(event, status, index) {
      this.items[index].isfavorite = status;
      console.log("favorite product=>", event, "status=>", status);
    }
  }
};
</script>

<style>
#app {
  font-family: "Roboto", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
