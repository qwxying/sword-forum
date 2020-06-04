<template>
  <div class="header">
    <img src="../assets/cnodejs_light.svg" alt="剑锋社区" />
    <ul>
      <li><a href="#">首页</a></li>
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
  name: "Header"
};
</script>

<style scoped>
.header {
  background-color: #444444;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

img {
  max-width: 120px;
  margin: 10px 50px;
}

ul {
  list-style: none;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li {
  list-style: none;
  text-align: center;
  padding: 5px 15px;
}

a {
  text-decoration: none;
  color: #cccccc;
  font-size: 14px;
  text-shadow: none;
}
</style>
