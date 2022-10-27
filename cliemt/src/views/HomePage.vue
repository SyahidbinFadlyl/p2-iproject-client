<script>
  import { mapActions, mapState } from "pinia";
  import { useCounterStore } from "../stores/counter";
  import CardPost from "../components/CardPost.vue";
  import infiniteScroll from "vue-infinite-scroll";

  export default {
    components: {
      CardPost,
    },
    methods: {
      ...mapActions(useCounterStore, ["readAllPost"]),
      loadMore: function () {
        this.busy = true;

        setTimeout(() => {
          for (var i = 0, j = 10; i < j; i++) {
            this.data.push({ name: count++ });
          }
          this.busy = false;
        }, 1000);
      },
      handleScrollToBottom() {
        console.log("abc");
      },
    },
    created() {
      this.readAllPost();
    },
    computed: {
      ...mapState(useCounterStore, ["allPost"]),
    },
  };
</script>

<template>
  <div id="container">
    <CardPost v-for="post in allPost" :key="post.id" :post="post" />
  </div>
  <!-- <div
    v-infinite-scroll="loadMore"
    infinite-scroll-disabled="busy"
    infinite-scroll-distance="10"
  >
    ...
  </div> -->
  <!-- <div v-observe-visibility="handleScrollToBottom"></div> -->
</template>

<style scoped>
  #container {
    margin-top: 50px;
  }
</style>
