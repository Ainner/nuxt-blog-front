<template>
  <div>
    <a-menu v-model="current" mode="horizontal">
      <a-menu-item key="mail"><nuxt-link to="/404">404</nuxt-link></a-menu-item>
      <a-dropdown :trigger="['click']">
        <a class="ant-dropdown-link" @click="e => e.preventDefault()">
          下拉菜单 <a-icon type="down" />
        </a>
        <a-menu slot="overlay">
          <a-menu-item key="0">
            <nuxt-link to="/">首页</nuxt-link>
          </a-menu-item>
          <a-menu-item key="1">
            <nuxt-link to="/404">404</nuxt-link>
          </a-menu-item>
          <a-menu-divider />
          <a-menu-item key="3">
            3rd menu item
          </a-menu-item>
        </a-menu>
      </a-dropdown>
    </a-menu>
  </div>
</template>
<script>
export default {
  data() {
    return {
      current: ['mail'],
    };
  },
};
</script>