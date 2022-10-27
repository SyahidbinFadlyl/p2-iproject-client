<script>
  import { mapActions, mapState } from "pinia";
  import { useCounterStore } from "../stores/counter";
  export default {
    data() {
      return {
        caption: "",
        content: "Caption..",
        imgUrl: null,
        previewImage: null,
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
      selectImage() {
        this.$refs.fileInput.click();
      },
      pickFile() {
        let input = this.$refs.fileInput;
        let file = input.files;
        document.getElementsByClassName("imagePreview")[0].style.width =
          "500px";
        document.getElementsByClassName("imagePreview")[0].style.height =
          "500px";
        if (file && file[0]) {
          let reader = new FileReader();
          reader.onload = (e) => {
            this.previewImage = e.target.result;
          };
          reader.readAsDataURL(file[0]);
          this.$emit("input", file[0]);
        }
      },
    },
  };
</script>

<template>
  å
  <div class="containerCreatePost">
    <div>
      <input
        type="text"
        name=""
        id="title"
        placeholder="Title.."
        v-model="caption"
      />
    </div>
    <div>
      <textarea name="content" id="" cols="30" rows="10" v-model="content">{{
        this.content
      }}</textarea>
    </div>
    <div>
      <div
        class="imagePreview"
        :style="{ 'background-image': `url(${previewImage})` }"
        @click="selectImage"
      ></div>
      <input
        type="file"
        @change="previewFiles"
        name="imgUrl"
        id="imgUrl"
        required
        @input="pickFile"
        ref="fileInput"
      />
    </div>
    <button @click="onUpload">Post</button>
  </div>
</template>

<style scoped>
  .containerCreatePost {
    width: 100%;
    text-align: center;
    align-items: center;
    justify-content: center;
    margin: auto;
    width: 50%;
    padding: 10px;
    margin-top: 5rem;
  }
  input[type="text"],
  textarea,
  input[type="file"] {
    width: 265px;
    padding: 12px 20px;
    margin: 8px 0;
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }
  button {
    width: 265px;
    padding: 12px 20px;
    color: white;
    background-color: rgb(200, 0, 0);
    border-radius: 10px;
    border: none;
    cursor: pointer;
    margin-top: 1rem;
  }
  .imagePreview {
    /* width: 250px;
    height: 250px; */
    display: block;
    cursor: pointer;
    margin: 0 auto 30px;
    background-position: center;
    object-fit: contain;
    background-size: contain;
    background-repeat: no-repeat;
  }
</style>
