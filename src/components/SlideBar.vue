<template>
  <div class="author-info">
    <div class="author-summary">
      <div class="top-bar">作者</div>
      <router-link
        :to="{
          name: 'user_info',
          params: {
            name: userInfo.loginname
          }
        }"
      >
        <img :src="userInfo.avatar_url" alt="" />
      </router-link>
    </div>
    <div class="recent-topics">
      <div class="top-bar">作者最近主题</div>
      <ul>
        <li v-for="list in topic_limit">
          <router-link
            :to="{
              name: 'post_content',
              params: {
                id: list.id,
                name: list.author.loginname
              }
            }"
          >
            {{ list.title }}
          </router-link>
        </li>
      </ul>
    </div>
    <div class="recent-replies">
      <div class="top-bar">作者最近回复</div>
      <ul>
        <li v-for="list in reply_limit">
          <router-link
            :to="{
              name: 'post_content',
              params: {
                id: list.id,
                name: list.author.loginname
              }
            }"
          >
            {{ list.title }}
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "SlideBar",
  data() {
    return {
      userInfo: {}
    };
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          this.isLoading = false; //加载成功，不渲染gif
          this.userInfo = res.data.data;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  computed: {
    topic_limit() {
      if (this.userInfo.recent_topics.length) {
        return this.userInfo.recent_topics.slice(0, 5);
      }
    },
    reply_limit() {
      if (this.userInfo.recent_replies.length) {
        return this.userInfo.recent_replies.slice(0, 5);
      }
    }
  },
  beforeMount() {
    this.isLoading = true; //加载成功之前显示gif
    this.getData(); //先获取数据
  }
};
</script>

<style scoped>
.author-summary,
.recent_replies,
.recent_topics {
  background-color: #fff;
}
.author-info {
  width: 328px;
  float: right;
  margin-top: 0;
}
li {
  padding: 3px 0;
}
.recent-replies ul,
.recent-topics ul {
  margin-top: 0;
  margin-bottom: 0;
  list-style: none;
  padding-left: 14px;
}

ul a {
  font-size: 12px;
  text-decoration: none;
  color: #778087;
}

.top-bar {
  padding: 10px;
  background-color: #f6f6f6;
  height: 16px;
  font-size: 12px;
  margin-top: 10px;
}

img {
  height: 48px;
  width: 48px;
  border-radius: 3px;
  margin: 10px;
}

.login-name {
  width: 100px;
  float: right;
  margin-top: 22px;
  margin-right: 159px;
  font-size: 14px;
}

.login-name a {
  text-decoration: none;
  color: #778087;
}

.author-summary .topbar {
  margin-top: 0;
}
</style>
