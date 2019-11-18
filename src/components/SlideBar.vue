<template>
  <div class="authorinfo">
    <div class="author_summary">
      <div class="topbar">作者</div>
      <router-link
        :to="{
        name: 'user_info',
        params: {
          name: userinfo.loginname,
        }
      }"
      >
        <img :src="userinfo.avatar_url" alt />
        <span>{{userinfo.loginname}}</span>
      </router-link>
    </div>
    <div class="recent_topics">
      <div class="topbar">作者最近主题</div>
      <ul>
        <li v-for="list in topicLimit" :key="list.id">
          <router-link :to="{
            name: 'post_content',
            params: {
              id: list.id,
              name: list.author.loginname,
            }
          }">
            {{list.title}}
          </router-link>
          </li>
      </ul>
    </div>
    <div class="recent_replies">
      <div class="topbar">作者最近回复</div>
      <ul>
        <li v-for="list in replyLimit" :key="list.id">
          <router-link :to="{
            name: 'post_content',
            params: {
              id: list.id,
              name: list.author.loginname,
            }
          }">
            {{list.title}}
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
      userinfo: {},
      isLoading: true
    };
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          this.isLoading = false;
          this.userinfo = res.data.data;
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  beforeMount() {
    this.getData();
  },
  computed: {
    topicLimit() {
      if (this.userinfo.recent_topics) {
        return this.userinfo.recent_topics.slice(0, 5);
      }
    },
    replyLimit() {
      if (this.userinfo.recent_replies) {
        return this.userinfo.recent_replies.slice(0, 5);
      }
    }
  }
};
</script>

<style scoped>
.author_summary,
.recent_replies,
.recent_topics {
  background-color: #fff;
  border-radius: 5px;
  margin-bottom: 10px;
  font-size: 14px;
}
.author_summary span {
  color: #778087;
}
.authorinfo {
  width: 328px;
  float: right;
  margin-top: -10px;
}
li {
  padding: 3px 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.recent_replies ul,
.recent_topics ul {
  margin-top: 0px;
  margin-bottom: 0px;
  list-style: none;
  padding: 5px 15px;
}

ul a {
  font-size: 12px;
  text-decoration: none;
  color: #778087;
}

.topbar {
  background-color: #f6f6f6;
  height: 30px;
  font-size: 12px;
  line-height: 30px;
}

img {
  height: 48px;
  width: 48px;
  border-radius: 3px;
  margin: 10px;
  vertical-align: middle;
}

.loginname {
  width: 100px;
  float: right;
  margin-top: 22px;
  margin-right: 159px;
  font-size: 14px;
}

.loginname a {
  text-decoration: none;
  color: #778087;
}

.authorsummary .topbar {
  margin-top: 0px;
}
</style>