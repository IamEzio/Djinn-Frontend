<template>
  <div class="page-search">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">Search</h1>
        <h2 class="is-size-5 has-text-grey">Search term : {{ query }}</h2>
      </div>
      <product-box
        v-for="product in products"
        :key="product.id"
        :product="product"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ProductBox from "../components/ProductBox.vue";
export default {
  components: { ProductBox },
  name: "Search",
  data() {
    return {
      products: [],
      query: "",
    };
  },
  methods: {
    async performSearch() {
      this.$store.commit("setIsLoading", true);

      await axios
        .post("/api/v1/products/search/", { query: this.query })
        .then((response) => {
          this.products = response.data;
        })
        .catch((error) => {
          console.log(error);
        });

      this.$store.commit("setIsLoading", false);
    },
  },
  mounted() {
    document.title = "Search | Djinn";
    let url = window.location.search.substring(1);
    let params = new URLSearchParams(url);
    if (params.get("query")) {
      this.query = params.get("query");
      this.performSearch();
    }
  },
};
</script>

<style lang="scss" scoped></style>
