<template>
  <div class='PostList'>
    <!-- 数据未返回时显示正在加载的gif -->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="图片" />
    </div>
    <div class="posts" v-else>
      <ul>
        <div class='toobar'>
        <li>
          <span>全部</span>
          <span>精华</span>
          <span>分享</span>
          <span>问答</span>
          <span>招聘</span>
        </li>
        </div>
       <li v-for="post in posts">
          <!-- 头像 -->
          <img :src="post.author.avatar_url" alt="">
          <span class='allcount'>
            <span class='reply_count'>{{post.reply_count}}</span>
            /{{post.visit_count}}
          </span>
          <!-- 帖子的分类 -->
          <span :class="[{put_tood: (post.good === true), put_top: (post.top === true), 'topiclist-tab': (post.good !== true && post.top !== true)}]">
            {{post | tabFormatter}}
          </span>
          <!-- 标题 -->
          <span class='title'>
            {{post.title}}
          </span>
          <!-- 最终回复时间 -->
          <span  class='last-reply'>
            {{post.last_reply_at | formatDate}}
          </span>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  name: "PostList",
  data() {
    return {
      isLoading: false,
      posts: []
    };
  },
  methods: {
    getData() {
      this.$http
        .get("https://cnodejs.org/api/v1/topics", {
          page: 1,
          limit: 20
        })
        .then(res => {
          this.isLoading = false;
          this.posts = res.data.data;
          console.log(res.data);
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  beforeMount() {
    this.isLoading = true; //在页面加载之前显示加载动画
    this.getData(); //在页面加载之前获取数据
  }
};
</script>
<style scoped>
* {margin: 0;padding: 0; box-sizing: border-box;}
  .PostList{
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
    width: 80%;
    max-width: 1344px;
    margin: 0 auto;
  }

  ul li:not(:first-child) {
    padding: 9px;
    font-size: 15px;
    font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
    font-weight: 400;
    background-color: white;
    color: #333;
    border-top: 1px solid #f0f0f0;
  }

  li:not(:first-child):hover {
    background: #f5f5f5;;
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

  .put_good, .put_top {
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

  .topiclist-tab {
    background-color: #e5e5e5;
    color: #999;
    padding: 2px 4px;
    border-radius: 3px;
    font-size: 12px;
    margin-right: 10px;
  }

  .last-reply {
    text-align: right;
    min-width: 50px;
    white-space: nowrap;
    color: #778087;
    font-size: 12px;
    display: block;
    float: right;
  }

  .toobar {
    height: 40px;
    background-color: #f5f5f5;
  }

  .toobar span {
    font-size: 14px;
    color: #80bd01;
    line-height: 40px;
    margin: 0 10px;
    cursor: pointer;
  }

  .toobar span:hover {
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
  .title {
    text-overflow: ellipsis;
  }
</style>