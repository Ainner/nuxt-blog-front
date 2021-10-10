<template>
  <div>
    <ul id="custom_menu">
      <li><nuxt-link to="/">首页</nuxt-link></li>
      <li><nuxt-link to="/todo/">待办</nuxt-link></li>
      <li><nuxt-link to="/bookmarks/">书签</nuxt-link></li>
      <li><nuxt-link to="/404/">错误</nuxt-link></li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
    };
  },
};
</script>

<style scoped>
#custom_menu {
  list-style: none;
  margin: 0;
  color: #333;
}
#custom_menu li {
  float: left;
  margin-left: 40px;
}

</style>