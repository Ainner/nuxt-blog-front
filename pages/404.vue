<template>
  <div>
    <h1><nuxt-link to="/">回到首页! 👋</nuxt-link></h1>
  </div>
</template>
<script>
export default {
  layout: 'index',
  head: {
    title: "无",
    meta: [
      {
        hid: "description",
        name: "description",
        content:
          "无",
      },
    ],
  },
}
</script>