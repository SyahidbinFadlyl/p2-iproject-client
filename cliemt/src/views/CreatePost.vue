<script>
  import { mapActions, mapState } from "pinia";
  import { useCounterStore } from "../stores/counter";
  export default {
    data() {
      return {
        caption: "",
        content: "",
        imgUrl: null,
      };
    },
    methods: {
      ...mapActions(useCounterStore, ["createNewPost"]),
      previewFiles(event) {
        console.log(event.target.files[0]);
        this.imgUrl = event.target.files[0];
      },
      onUpload() {
        this.createNewPost({
          caption: this.caption,
          content: this.content,
          imgUrl: this.imgUrl,
        });
      },
      resetImageUploader() {
        console.log("masuk");
        this.imgUrl = null;
      },
    },
  };
</script>

<template>
  <h1>halaman create post</h1>
  <div>
    <input type="text" name="" id="" placeholder="Judul.." v-model="caption" />
  </div>
  <div>
    <input type="text" placeholder="Caption.." v-model="content" />
  </div>
  <div>
    <input
      type="file"
      @change="previewFiles"
      name="imgUrl"
      id="imgUrl"
      required
      @click="resetImageUploader"
    />
  </div>
  <button @click="onUpload">Post</button>
</template>
