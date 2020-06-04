<template>
  <div class="article">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="加载中…" />
    </div>
    <div v-else>
      <div class="topic_header">
        <div class="topic_title">{{ post.title }}</div>
        <ul>
          <li>· 发布于：{{ post.create_at | formatDate }}</li>
          <li>· 作者：{{ post.author.loginname }}</li>
          <li>· {{ post.visit_count }}次浏览</li>
          <li>· 来自：{{ post | tabFormatter }}</li>
        </ul>
        <div class="topic_content" v-html="post.content"></div>
      </div>
      <div class="topbar">回复</div>
      <div id="reply" v-for="(reply, index) in post.replies" class="replySec">
        <div class="replyUp">
          <router-link
            :to="{
              name: 'user_info',
              params: {
                name: reply.author.loginname
              }
            }"
          >
            <img :src="reply.author.avatar_url" alt="" />
            <span>{{ reply.author.loginname }}</span>
          </router-link>

          <span>{{ index + 1 }}楼 </span>
          <span v-if="reply.ups.length > 0">👍{{ reply.ups.length }}</span>
        </div>
        <p v-html="reply.content"></p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Article",
  data() {
    return {
      isLoading: false,
      post: {}
    };
  },
  methods: {
    getArticleData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
        .then(res => {
          if (res.data.success) {
            this.isLoading = false;
            this.post = res.data.data;
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  watch: {
    $route() {
      this.getArticleData();
    }
  },
  beforeMount() {
    this.isLoading = true;
    this.getArticleData();
  }
};
</script>

<style>
@import url("../assets/markdown-github.css");

.topbar {
  padding: 10px;
  background-color: #f6f6f6;
  height: 16px;
  font-size: 12px;
  margin-top: 10px;
}

.article:not(:first-child) {
  margin-right: 340px;
  margin-top: 15px;
}

#reply,
.topic_header {
  background-color: #fff;
}

#reply {
  margin-top: 15px;
}

#reply img {
  width: 30px;
  height: 30px;
  position: relative;
  bottom: -9px;
}

#reply a,
#reply span {
  font-size: 13px;
  color: #666;
  text-decoration: none;
}

.replySec {
  border-bottom: 1px solid #e5e5e5;
  padding: 0 10px;
}

.loading {
  text-align: center;
  padding-top: 300px;
}

.replyUp a:nth-of-type(2) {
  margin-left: 0;
  display: inline-block;
}

.topic_header {
  padding: 10px;
}

.topic_title {
  font-size: 20px;
  font-weight: bold;
  padding-top: 8px;
}

.topic_header ul {
  list-style: none;
  padding: 0 0;
  margin: 6px 0;
}

.topic_header li {
  display: inline-block;
  font-size: 12px;
  color: #838383;
}

.topic_content {
  border-top: 1px solid #e5e5e5;
  padding: 0 10px;
}

.markdown-text img {
  width: 92% !important;
}
</style>
