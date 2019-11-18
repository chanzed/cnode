<template>
  <div class="article">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="图片" />
    </div>
    <div v-else>
      <div class="topic_header" id="content">
        <div class="topic_title">{{post.title}}</div>
        <ul>
          <li>●&nbsp;发布于&nbsp;{{post.create_at | formatDate}}</li>
          <li>●&nbsp;作者&nbsp;{{post.author.loginname}}</li>
          <li>●&nbsp;{{post.visit_count}}次浏览</li>
          <li>●&nbsp;来自&nbsp;{{post | tabFormatter}}</li>
        </ul>
        <div v-html="post.content" class="topic_content"></div>
      </div>
    </div>
    <div id="reply">
      <div class="topbar" v-if="post.replies">{{post.replies.length}}回复</div>
      <div v-for="(reply, index) in post.replies" :key="index" class="replySec">
        <div class="replyUp">
          <router-link
            :to="{
            name: 'user_info',
            params: {
              name: reply.author.loginname,
            }
          }"
          >   
            <img :src="reply.author.avatar_url" alt />
          </router-link>  
          <router-link
            :to="{
            name: 'user_info',
            params: {
              name: reply.author.loginname,
            }
          }"
          >
            <span>{{reply.author.loginname}}</span>
          </router-link>
          <span>{{index+1}}楼</span>
        </div>
        <p v-html="reply.content"></p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isLoading: false,
      post: {}
    };
  },
  methods: {
    getArticleData() {
      this.$http
        .get("https://cnodejs.org/api/v1/topic/" + this.$route.params.id)
        .then(res => {
          if (res.data.success === true) {
            this.isLoading = false;
            this.post = res.data.data;
            console.log(this.post.replies)
          }
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  beforeMount() {
    this.isLoading = true;
    this.getArticleData();
  },
  watch: {
    '$route': 'getArticleData',
  }
};
</script>
<style>
@import url(../assets/markdown-github.css);
.topbar {
  background-color: #f6f6f6;
  height: 30px;
  font-size: 16px;
  margin-top: 10px;
  line-height: 30px;
  padding-left: 10px;
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
  vertical-align: top;
  border-radius: 3px;
}

#reply a,
#reply span {
  font-size: 13px;
  color: #666;
  text-decoration: none;
}
.replySec {
  border-bottom: 1px solid #e5e5e5;
  padding: 10px 10px;
  margin-top: 10px 0;
}
.replySec p {
  font-size: 14px;
  line-height: 24px;
  margin-left: 1em;
}

.loading {
  text-align: center;
  padding-top: 300px;
}
.replyUp a:nth-of-type(2) {
  margin-left: 0px;
  display: inline-block;
  margin: 10px 0;
}

.topic_header {
  padding: 10px;
  margin-top: 15px;
}

.topic_title {
  font-size: 20px;
  font-weight: bold;
  padding-top: 8px;
}

.topic_header ul {
  list-style: none;
  padding: 0px 0px;
  margin: 6px 0px;
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
</style>