<template>
  <div style="display: flex;justify-content: space-evenly;">

  <a-card v-for="(item, index) in article" :key="index" hoverable style="width: 300px">
    <img
      slot="cover"
      alt="example"
      :src="item.img_url"
    />
    <template slot="actions" class="ant-card-actions">
      <a-icon key="setting" type="setting" />
      <nuxt-link :to="{ name: 'wakagi-posts-add', params: { id: item._id }}"><a-icon key="edit" type="edit" /></nuxt-link>
      <a-icon key="ellipsis" type="ellipsis" />
    </template>
    <a-card-meta :title="item.title" :description="item.desc">
      <a-avatar
        slot="avatar"
        src="http://w-ww.cn-sh2.ufileos.com/logo.png"
      />
    </a-card-meta>
  </a-card>
 

  </div>
</template>
<script>
export default {
  layout: 'wakagi',
  data() {
    return {
      article: [],
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
    this.getArticleListAdmin()
  },
  methods: {
    getArticleListAdmin() {
      fetch('http://chimeiwangliang.cn/api/getArticleListAdmin', {
        method: 'GET',
      })
      .then(response => {
        if(response.ok) {
          response.json().then(data => {
            this.article = data.data.list
          })
        }
      })
      .catch(error => console.log(error))
    }
  },
};
</script>

<style scoped>
div.signup {
    display: none;
}
</style>