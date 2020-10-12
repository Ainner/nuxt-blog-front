<template>
  <div>
    <a-page-header
      style="border: 1px solid rgb(235, 237, 240)"
      title="Title"
      :breadcrumb="{ props: { routes } }"
      sub-title="This is a subtitle"
    />
    <a-card :loading="loading" title="Card title">
      whatever content
    </a-card>
  </div>
</template>
<script>
export default {
  data() {
    return {
      loading: true,
      routes: [
        {
          path: 'index',
          breadcrumbName: 'First-level Menu',
        },
        {
          path: 'first',
          breadcrumbName: 'Second-level Menu',
        },
        {
          path: 'second',
          breadcrumbName: 'Third-level Menu',
        },
      ],
    };
  },
  head: {
    title: 'Home page',
    meta: [
      { 
        hid: 'description', 
				name: 'description', 
				content: 'The amazing Nuxt.js application that teaches me all the cool features of Nuxt.js with a Hello World' }
    ],
  },
  mounted () {
    this.$nextTick(() => {
      this.$nuxt.$loading.start()
      setTimeout(() => this.$nuxt.$loading.finish(), 2000)
    })
  },
  methods: {
    onChange(a, b, c) {
      console.log(a, b, c);
    },
  },
}
</script>

<style scoped>
.ant-page-header.has-breadcrumb {
  background-color: #ffffffe0;
}
</style>