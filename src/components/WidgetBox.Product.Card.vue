<template>
  <div class="widget-container">
    <div
      :class="'widget-card widget-elevation-' + options.elevation"
      :style="{ background: options.color }"
      v-for="(item, index) in items"
    >
      <div class="wimg" :style="'background:url(' + item.image + ')'">
        <div
          v-if="options.salebox && parseFloat(item.sale_price) > 0"
          class="widget-sale"
        >
          SALE
        </div>
      </div>
      <div class="widget-title">
        <div :style="{ color: options.text_color }">{{ item.label }}</div>
        <div :style="{ color: options.title_color }" class="product-title">
          {{ item.name }}
        </div>
      </div>
      <!--
        <hr class="divider" :style="{ background: options.text_color }" />
      -->
      <div class="widget-card-footer">
        <div class="widget-rating">
          <star-rating
            :rating="item.rating"
            :star-size="15"
            :show-rating="false"
            :active-color="options.rating_color"
            @rating-selected="setRating($event, item);"
          ></star-rating>
        </div>
        <div
          class="widget-button"
          @click="setFavorite(item, !item.isfavorite, index);"
        >
          <i
            class="material-icons widget-icon"
            :style="{ color: options.icons_color }"
            v-if="!item.isfavorite"
            >favorite_border</i
          >
          <i
            class="material-icons widget-icon"
            :style="{ color: options.icons_color }"
            v-if="item.isfavorite"
            >favorite</i
          >
        </div>
        <div class="widget-button">
          <i
            class="material-icons widget-icon"
            :style="{ color: options.icons_color }"
            @click="addToCart(item);"
            >shopping_cart</i
          >
        </div>
        <div class="widget-price">
          <div
            v-if="parseFloat(item.sale_price) === 0"
            :class="'product-price'"
            :style="{ color: options.price_color }"
          >
            {{ options.currency }}{{ fullprice(item.price, item.sale_price) }}
          </div>
          <div
            class="product-sale"
            :style="{ color: options.sale_color }"
            v-if="parseFloat(item.sale_price) > 0"
          >
            {{ options.currency }}{{ item.sale_price.toFixed(2) }}
            <span :style="{ color: options.price_color }" class="fullprice">{{
              item.price.toFixed(2)
            }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import StarRating from "vue-star-rating";

export default {
  components: {
    StarRating
  },

  props: {
    items: { type: Array, required: true },
    options: {
      type: Object,
      required: false,
      default: {
        color: "#fff",
        title_color: "#555",
        text_color: "#ccc",
        price_color: "#555",
        sale_color: "#ff0000",
        icons_color: "#ccc",
        rating_color: "#ffd055",
        rating: true,
        favorite: true,
        currency: "$",
        salebox: false
      }
    }
  },
  methods: {
    addToCart(product) {
      this.$emit("widgetClick", product);
    },
    setRating(rating, id) {
      this.$emit("widgetRating", rating, id);
    },
    setFavorite(product, status, i) {
      this.$emit("widgetFavorite", product, status, i);
    },
    setIcon() {
      if (this.isfavorite) {
        return "favorite";
      } else {
        return "favorite_border";
      }
    },
    fullprice(price, sale_price) {
      if (parseFloat(sale_price) === 0) {
        return parseFloat(price).toFixed(2);
      } else {
        this.strikethru = "fullprice";
        return parseFloat(price).toFixed(2);
      }
    }
  },
  data: () => ({
    fullscreen: false,
    strikethru: "",
    pimage:
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAOEAAADhCAMAAAAJbSJIAAAANlBMVEX///+/v7+8vLzKysr8/Pzg4ODFxcXb29vT09Pj4+P4+PjBwcH6+vry8vLY2NjIyMjp6eno6Og1AQOEAAAEdUlEQVR4nO2d6ZbCIAxGp3TT1i6+/8uOWrUUwlKsIZ7z3d+dwh0wIYDHvz8AAAAAAAAAAAAAAAAAXqqx+Snq/YYnVfwQqoUhDKUDQxjKB4YwlA8MYSifAwyVNA43PJ2FUR5sqM673/BljP7B0AaG2YFhEBhmB4ZBYJgdGAaBYXZgGASG2YFhEBhmB4ZBYJidDIaXKqWjyfAaXubxsUs7nbvkHu+F1XAuX1vQqmm5RpLTsO61R9WJaRgZDc0Djmb4pOPR8BmezTMqVX7U81jYDIemMFH775glwGY4EseMH8zTNvpPuQw7ewhvJC8Phn6MfZTLcKZOitUpMWXcGp1iQzGXITVJiyK6mwatih9/LkP6tL9PM+zUjvH/ScPlZZGxJq9hk2RYP96lTnFPcxnWpGF52d2ellnjHucyHMhYGh3ydcrX6v0a9TiX4WV76WOhn3c3p8+GMirWsK1pztQkTUiH2uov7h/EZliV1jxVCYu26rS3t3y1xaU3DVMW3puIFRWKGevDudmOYkqV3xWRja1w1viDNlFV2qackVab3X/y7Z2oc9k8dqL6aUzJhH+1Mc9jPsnMu4nVULfjWF/TVmtWFR2TUH9qR9gOxxHFiTDD2Rd9qJVfOB7LMuzU6FYkdnpiVrayDG/vclYMeq7XCMYaUYb3lZ2zsqWrk3ANJcmwmx6voCfqNtdrDYbWDZIMX3s55LC4vvQQbFCQ4buEpMKNmetXQgWKHEO9grQUyTi6EKqh5BhuCkhzrWLn+rXFwLpGjOHQb7u9GUXrVGcziP6EIcbQHCW9tvLM0SJ4wCPF0N71XyefI9e/8ddQQgyJg5v180Xneu1Jb6wRYthSPX92xpXr1za96xoZhoO1h6MpunL9ivccktuwIgu6yTE49+64c/36nC/WcBteS0LRnQzaQBx94quhuHcxSmX3pnMM4Z06PEcL/34Ns+Ft4aJ6Q9H7TeIYvxueax3Mt77ua09lTNQ5TsKLck9TXsNnXt8qxnzQgobuWMNr+EwKSj/7bSMnop/JOYishtd3BVi+Q0NUrAzTOw8TOQ0rrQJ8TdTQmjMW98UFTkO9AnxNVG9dtAdnDcVouD0GVtO9SxW9XEvA2XXO0zVjuO478seEmQd9fkN7uLr5sCF0X1zgM7zawzVR1xeSDR01FOM5/oE2JI4aKutdjGNxdD7rfZqDoQ8Ts94vPRh6v4bL8MCg6WbKaEgE0i+gqGnKY/j9QOruPo/h9wPpAnVJisWQI5AuEK2zGHIE0qV1ooZiMWQJpAv2uobDkCeQLs3bh4kMhkyB9EkOw8PK+Bjs9r9vWI0lJ9Y0ZRjDjhVrV1HG6do3gWEQGGYHhkFgmB0YBoFhdmAYBIbZOdqwqCtZHG/YsFaDEWw3ifDbCDCUDwxhKB8YwlA+MIShfGAIQ/nAEIbygSEM5QNDGMoHhjCUDwxhKB8YwlA+MIShfGAIQ/nAEIbygSEM5QNDGMoHhjCUT5rhT5FgONc/Bc/PuwIAAAAAAAAAAAAAAAAQzD8O4oElgraltwAAAABJRU5ErkJggg=="
  }),
  mounted() {
    if (this.image) {
      this.pimage = this.image;
    }
  }
};
</script>
<style>
.widget-container {
  max-width: 100%;
  display: grid;
  justify-content: space-evenly;
  grid-gap: 0.5rem;
  grid-template-columns: repeat(auto-fill, 320px);
  margin-bottom: 2rem;
}

.widget-card {
  display: block;
  border: 0px solid #eaeaea;
  width: 100%;
  margin-bottom: 1rem;
}

.widget-card > .wimg {
  position: absolute;
  margin: 1rem 0 0 1rem;
  border-radius: 0.2rem;
  border: 1px solid #eaeaea;
  max-width: 8rem;
  width: 8rem;
  height: 8rem;
  max-height: 8rem;
  background-size: cover !important;
  background-position: center center !important;
  background-repeat: no-repeat !important;
}

.widget-sale {
  position: absolute;
  padding: 0.6rem;
  z-index: 1;
  background: red;
  font-size: 0.7rem;
  color: #fff;
}

.widget-title {
  min-height: 8rem;
  padding: 1rem;
  text-align: right;
  border-bottom: 1px solid #dfdfdf;
}

.widget-card-footer {
  max-width: 100%;
  margin-top: 1.5rem;
  min-height: 2.5rem;
  padding-bottom: 0.2rem;
  display: grid;
  justify-content: space-evenly;
  grid-gap: 0.002rem;
  grid-template-columns: 1fr 0.5fr 1fr 2fr;
}

.widget-rating {
  text-align: left;
  margin-left: 1rem;
  margin-right: 1rem;
  margin-top: 0.4rem;
}

.widget-button {
  text-align: center;
  cursor: pointer;
}

.widget-icon {
  border-radius: 50%;
  padding: 0.4rem;
}

.widget-icon:hover {
  background: #cecece;
}

.widget-price {
  text-align: center;
  padding: 0.3rem;
}

.widget-elevation-1 {
  box-shadow: 0 3px 1px -2px rgba(0, 0, 0, 0.2), 0 2px 2px 0 rgba(0, 0, 0, 0.14),
    0 1px 5px 0 rgba(0, 0, 0, 0.12) !important;
}
.widgetproduct {
  position: absolute;
  top: 1rem;
  left: 1rem;
  background: #fff;
  border: 1px solid #eaeaea;
  padding: 0;
  min-width: 6rem;
  width: 6rem;
  border-radius: 0.5rem;
}
.widgetproduct img {
  width: 100%;
  height: auto;
}

.product-title {
  font-size: 1.3rem;
}
.product-price {
  font-size: 1.2rem;
}

.product-sale {
  color: red;
  font-size: 1.2rem;
}
.fullprice {
  text-decoration: line-through;
  font-size: 0.8rem;
  margin-top: -1rem;
}
</style>
