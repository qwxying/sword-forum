<template>
  <div class="PostList">
    <!--    未返回数据时，显示正在加载的gig-->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="加载中，请稍候……" />
    </div>
    <!--    帖子列表-->
    <div class="posts" v-else>
      <ul>
        <li>
          <div class="topbar">
            <span>全部</span>
            <span>精华</span>
            <span>分享</span>
            <span>问答</span><span>招聘</span>
          </div>
        </li>
        <li v-for="post in posts">
          <!--          头像-->
          <img :src="post.author.avatar_url" alt="" />
          <!--          回复/浏览量-->
          <span class="allcount">
            <span class="reply_count">{{ post.reply_count }}</span>
            <span>/{{ post.visit_count }}</span>
          </span>
          <!--          帖子分类-->
          <span
            :class="[
              { put_good: post.good },
              { put_top: post.top },
              { topiclist_tab: !(post.good || post.top) }
            ]"
            >{{ post | tabFormatter }}
          </span>
          <!--          帖子标题-->
          <router-link
            :to="{
              name: 'post_content',
              params: { id: post.id, name: post.author.loginname }
            }"
          >
            <span class="title">
              {{ post.title }}
            </span>
          </router-link>
          <!--          最后回复时间-->
          <span class="last_reply">
            {{ post.last_reply_at | formatDate }}
          </span>
        </li>
        <li>
          <Pagination @handleList="renderList" />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import Pagination from "./Pagination";
export default {
  name: "PostList",
  data() {
    return {
      isLoading: false,
      posts: [], //帖子主题列表
      postpage: 1
    };
  },
  components: { Pagination },
  methods: {
    getData() {
      this.$http
        .get("https://cnodejs.org/api/v1/topics", {
          params: { page: this.postpage, limit: 20 }
        })
        .then(res => {
          this.isLoading = false; //加载成功，不渲染gif
          this.posts = res.data.data;
        })
        .catch(err => {
          console.log(err);
        });
    },
    renderList(value) {
      this.postpage = value;
      this.getData();
    }
  },
  beforeMount() {
    this.isLoading = true; //加载成功之前显示gif
    this.getData(); //先获取数据
  }
};
</script>

<style scoped>
.PostList {
  background-color: #e1e1e1;
}

.posts {
  margin-top: 10px;
}

.PostList img {
  height: 30px;
  width: 30px;
  vertical-align: middle;
}

ul {
  list-style: none;

  width: 100%;
  max-width: 1344px;
  margin: 0 auto;
  padding: 0;
}

ul li:not(:first-child) {
  padding: 9px;
  font-size: 15px;
  font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma,
    "Hiragino Sans GB", STHeiti, sans-serif !important;
  font-weight: 400;
  background-color: white;
  color: #333;
  border-top: 1px solid #f0f0f0;
}

li:not(:first-child):hover {
  background: #f5f5f5;
}

li:last-child:hover {
  background: white;
}

li span {
  line-height: 30px;
}

.allcount {
  width: 70px;
  display: inline-block;
  text-align: center;
  font-size: 12px;
}

.reply_count {
  color: #9e78c0;
  font-size: 14px;
}

.put_good,
.put_top {
  background: #80bd01;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  color: #fff;
  font-size: 12px;
  margin-right: 10px;
}

.topiclist_tab {
  background-color: #e5e5e5;
  color: #999;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  font-size: 12px;
  margin-right: 10px;
}

.last_reply {
  text-align: right;
  min-width: 50px;
  display: inline-block;
  white-space: nowrap;
  float: right;
  color: #778087;
  font-size: 12px;
}

.topbar {
  height: 40px;
  background-color: #f5f5f5;
}

.topbar span {
  font-size: 14px;
  color: #80bd01;
  line-height: 40px;
  margin: 0 10px;
  cursor: pointer;
}

.topbar span:hover {
  color: #9e78c0;
}

a {
  text-decoration: none;
  color: black;
}

a:hover {
  text-decoration: underline;
}

.loading {
  text-align: center;
  padding-top: 300px;
}
</style>
