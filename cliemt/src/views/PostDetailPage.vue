<script>
  import { mapActions, mapState } from "pinia";
  import { useCounterStore } from "../stores/counter";
  import CommentPost from "../components/CommentPost.vue";

  export default {
    data() {
      return {
        userId: localStorage.getItem("id"),
        liked: false,
        comment: "",
      };
    },
    components: {
      CommentPost,
    },
    methods: {
      ...mapActions(useCounterStore, [
        "readDataPostById",
        "addCommentToPost",
        "likePost",
        "unlikePost",
        "deletePost",
      ]),
      addComment() {
        this.addCommentToPost(this.$route.params.id, this.comment);
        this.comment = "";
      },
      likeThisPost() {
        this.likePost(this.$route.params.id);
        this.readDataPostById(this.$route.params.id);
      },
      unlikeThisPost() {
        this.unlikePost(this.$route.params.id);
        this.readDataPostById(this.$route.params.id);
      },
      deleteThisPost() {
        this.deletePost(this.$route.params.id);
      },
      handleLikePost(payload) {
        if (payload === "Unlike") {
          this.unlikePost(this.$route.params.id);
        } else {
          this.likePost(this.$route.params.id);
        }
      },
    },
    created() {
      this.readDataPostById(this.$route.params.id).then(() => {
        this.liked = dataPostById.post.liked;
      });
    },
    mounted() {},
    computed: {
      ...mapState(useCounterStore, ["dataPostById"]),
    },
  };
</script>

<template>
  <div v-if="dataPostById" class="container">
    <div class="imgContainer">
      <img class="imagePost" :src="dataPostById.post.imgUrl" alt="" />
    </div>
    <div class="detailPost">
      <div class="BTNLIKE">
        <button
          @click="deleteThisPost"
          v-if="dataPostById.post.UserId == this.userId"
        >
          Delete
        </button>
        <button
          id="buttonLikePost"
          @click="handleLikePost(dataPostById.post.liked)"
        >
          {{ dataPostById.post.liked }}
        </button>
        <!-- <button
          id="buttonLikePost"
          @click="likeThisPost"
          v-if="!dataPostById.post.liked && !this.liked"
        >
          Like
        </button> -->
      </div>
      <div class="postedBy">
        <div class="p1">di unggah oleh :</div>
        <div class="posterName">{{ dataPostById.post.User.username }}</div>
      </div>
      <div>
        <div class="postUser">
          <div>
            <img
              :src="dataPostById.post.User.photo"
              alt=""
              style="height: 40px; width: 40px; border-radius: 50%"
            />
          </div>
          <div class="userName">{{ dataPostById.post.User.email }}</div>
        </div>
      </div>
      <div class="contentPost">{{ dataPostById.post.caption }}</div>
      <div class="caption">
        {{ dataPostById.post.content }}
      </div>
      <div class="commentContainer">
        <div class="commentAreaContainer">
          <input
            class="commentArea"
            type="text"
            placeholder="comment here..."
            v-model="comment"
          />
          <button id="commentBTN" @click="addComment">Comment</button>
        </div>
        <div class="labelComment">KOMENTAR :</div>
        <div v-if="dataPostById.post.Comments.length > 0">
          <CommentPost
            v-for="comment in dataPostById.post.Comments"
            :key="comment.id"
            :comment="comment"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  ::placeholder {
    font-family: "Ubuntu Condensed", sans-serif;
    text-align: left;
    padding: 5px;
  }
  .commentAreaContainer {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
  }
  #commentBTN {
    background-color: white;
    color: black;
    height: 40px;
    font-weight: bold;
  }
  .commentArea {
    width: 80%;
    height: 30px;
    margin-left: auto;
    margin-left: auto;
    padding: 5px;
  }
  .BTNLIKE {
    display: flex;
    flex-direction: row;
    margin-top: 20px;
    margin-bottom: 40px;
  }
  button {
    display: block;
    margin-left: auto;
    margin-right: 0;
    height: 45px;
    width: 80px;
    border-radius: 40px;
    border: none;
    background-color: red;
    color: white;
    cursor: pointer;
    font-size: 16px;
    padding: 12px 16px;
  }
  .commentContainer {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    padding: 10px;
    justify-content: center;
  }
  .container {
    margin: auto;
    margin-top: 50px;
    /* width: 50%; */
    max-width: 1016px;
    padding: 20px;
    display: flex;
    flex-direction: row;
    justify-content: center;
    font-family: "Roboto", sans-serif;
    box-shadow: 12px 0 15px -4px rgba(31, 73, 125, 0.8),
      -12px 0 8px -4px rgba(31, 73, 125, 0.8);
    border-radius: 20px;
    background-color: white;
  }
  .postedBy {
    display: flex;
    flex-direction: column;
    font-size: 26px;
    padding: 10px 10px 10px 0;
    margin-top: 20px;
  }
  .postUser {
    display: flex;
    flex-direction: row;
    padding: 10px 10px 10px 0;
  }
  .posterName {
    font-weight: bold;
  }
  .contentPost {
    margin-top: 26px;
    font-size: 36px;
    text-align: left;
    font-weight: 600;
    padding-top: 10px;
    padding-bottom: 10px;
  }
  .imgContainer {
    display: block;
    margin: 20px 0;
    padding: 5px;
  }
  img {
    width: 508px;
    border-radius: 20px;
    margin-right: 5px;
  }
  .detailPost {
    height: 100%;
    padding: 0 30px 30px 30px;
    /* border: solid; */
    margin: 30px 0;
  }
  .userName {
    margin-top: auto;
    margin-bottom: auto;
    padding-left: 10px;
  }
  .labelComment {
    margin-top: 20px;
    padding: 10px 10px 10px 0;
    font-weight: bold;
  }
  .caption {
    margin: 10px 10px 10px 0;
    padding: 5px;
    font-size: 18px;
  }
  .commentList {
    display: flex;
    flex-direction: row;
    padding: 5px;
    align-items: top;
    margin-top: 10px;
  }

  .commentListUserComment {
    margin-left: 10px;
    padding-top: 5px;
  }
  p {
    padding: 0 5px 5px 0;
    font-size: 18px;
  }
  .commentListUserName {
    display: inline-block;
    font-size: 21px;
    font-weight: 600;
    padding-top: 5px;
    margin-right: 5px;
  }
  .commentListImg {
    padding-top: 5px;
  }
</style>
