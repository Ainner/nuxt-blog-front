<template>
  <div>
    <h1>这是标题</h1>
  </div>
</template>
<script>
export default {
  layout: 'post',
  name: 'postList',
  data() {
    return {
    };
  },
  head: {
    title: "文章列表",
    meta: [
      {
        hid: "description",
        name: "description",
        content:
          "文章列表",
      },
    ],
  },
  mounted() {
  },
  methods: {
  },
};
</script>

<style scoped>
div.signup {
    display: none;
}
</style>
