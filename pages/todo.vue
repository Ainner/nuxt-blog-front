<template>
  <div>
    <a-input placeholder="Basic usage" />

    <a-list bordered :data-source="news">
      <a-list-item slot="renderItem" slot-scope="item, index" :key="index">
        <a-checkbox @change="onChange">
          {{ item }} 
        </a-checkbox>
        <a-icon type="edit" /> <a-icon type="delete" />
      </a-list-item>
      <div slot="header">
        Header
      </div>
      <div slot="footer">
        Footer
      </div>
    </a-list>
    <div >
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      loading: false,
      news: [
        'Today is monday',
        'Today is monday',
        'Today is monday',
        'Today is monday',
        'Today is monday'
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