<template>
  <div>
    <div v-for="(item, index) in news" :key="index">
      <a-card :loading="loading" :title="item.title">{{ item.message }}</a-card>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      loading: false,
      news: [
        {
          title: "Today is monday.",
          message:
            "Labore amet elit aliquip ut voluptate ullamco laborum veniam duis id anim.",
        },
        {
          title: "Today is monday.",
          message:
            "Labore amet elit aliquip ut voluptate ullamco laborum veniam duis id anim.",
        },
        {
          title: "Today is monday.",
          message:
            "Labore amet elit aliquip ut voluptate ullamco laborum veniam duis id anim.",
        },
        {
          title: "Today is monday.",
          message:
            "Labore amet elit aliquip ut voluptate ullamco laborum veniam duis id anim.",
        },
      ],
      routes: [
        {
          path: "index",
          breadcrumbName: "First-level Menu",
        },
        {
          path: "first",
          breadcrumbName: "Second-level Menu",
        },
        {
          path: "second",
          breadcrumbName: "Third-level Menu",
        },
      ],
    };
  },
  head: {
    title: "Home page",
    meta: [
      {
        hid: "description",
        name: "description",
        content:
          "The amazing Nuxt.js application that teaches me all the cool features of Nuxt.js with a Hello World",
      },
    ],
  },
  mounted() {
    this.$nextTick(() => {
      this.$nuxt.$loading.start();
      setTimeout(() => this.$nuxt.$loading.finish(), 2000);
    });
  },
  methods: {
    onChange(a, b, c) {
      console.log(a, b, c);
    },
  },
};
</script>

<style scoped>
.ant-page-header.has-breadcrumb {
  background-color: #ffffffe0;
}
</style>