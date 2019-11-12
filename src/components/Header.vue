<template>
  <div class="header">
    <img src="../assets/cnodejs_light.svg" alt="">
    <ul>
      <li><a herf="#">首页</a></li>
      <li><a href="#">新手入门</a></li>
      <li><a href="#">API</a></li>
      <li><a href="#">关于</a></li>
      <li><a href="#">注册</a></li>
      <li><a href="#">登录</a></li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'Header',

}
</script>
<style scoped>
* {
  margin: 0px;
  padding: 0px;
}
.header {
  background-color: #444;
  height: 50px;
  margin: 0;
  padding: 0;
}
img {
  width: 120px;
  margin-left: 50px;
  margin-top: 10px;
}
ul {
  list-style: none;
  float: right;
  height: 50px;
  line-height: 50px;
}
li {
  display: inline-block;
  margin: 0 15px;
}
a {
  color: #ccc;
  text-decoration: none;
  font-size: 14px;
  text-shadow: none;
}
</style>