<template>
  <div>
    <Header>
      <template #middle>
        <div class="title d-flex justify-content-between align-items-center">
          <span class="title__chinese" />
          <span class="title__english" />
          <span class="title__chinese" />
        </div>
      </template>
    </Header>
    <div class="container">
      <div v-if="data.length" class="row mx-lg-5">
        <router-link
          v-for="item in data"
          :key="Object.values(item)[0]"
          :to="{ name: 'Tourism', params: { id: Object.values(item)[0] } }"
          class="col-xl-3 col-lg-4 col-md-6 col-sm-6 mb-4"
        >
          <Card :item="item" />
        </router-link>
      </div>
      <div v-else class="my-5">
        <h1>您沒有任何收藏</h1>
      </div>
    </div>
    <Footer />
  </div>
</template>

<style lang="scss" scoped>
.title {
  color: $Off_White;
  cursor: default;
  font-family: Noto Serif JP;
  &__chinese {
    font-size: 96px;
    font-weight: bold;
  }
  &__english {
    font-size: 32px;
    letter-spacing: 0.4em;
    margin-right: -0.4em;
  }
}
h1 {
  font-weight: 700;
  font-size: 40px;
  color: $Title_Active;
}
</style>

<script>
import Header from "@/components/Header.vue";
import Footer from "@/components/Footer.vue";
import Card from "@/components/Card.vue";
import { values } from "idb-keyval";

export default {
  name: "Home",
  components: {
    Header,
    Footer,
    Card,
  },
  data() {
    return {
      title: { ch: "收藏", en: "Collection", value: "Collection" },
      data: [],
    };
  },
  mounted() {
    navigator.onLine
      ? (this.data = this.$store.getters.getCollection)
      : values().then((values) => (this.data = values));
    [].forEach.call(
      document.querySelectorAll(".title__chinese"),
      (element, index) => (element.innerHTML = this.title.ch.substr(index, 1))
    );
    document.querySelector(".title__english").innerHTML = this.title.en;
  },
};
</script>
