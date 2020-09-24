<template>
  <div>
    <h1>Hello Nuxters! 👋</h1>
  </div>
</template>
<script>
export default {
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
  }
}
</script>
