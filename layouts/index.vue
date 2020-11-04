<template>
  <section style="display:flex">
    <TheHeader id="header" />
    <div id="content_left">
      <Nuxt id="main" />
    </div>
    <div id="content_right">
      <div>
        <div id="news">
          <div v-for="(item, index) in news" :key="index">
            <a-card style="margin-bottom:20px;" :loading="loading" :title="item.title.rendered"><div class="content" v-html="item.content.rendered"></div></a-card>
          </div>
        </div>
      </div>
    </div>
    <TheFooter id="footer" />
  </section>
</template>

<script>
export default {
  data() {
    return {
      loading: false,
      news: [],
    }
  },
  methods: {
    getNewsList() {
      fetch('http://test.chimeiwangliang.cn/wp-json/wp/v2/posts', {
        method: 'GET',
      })
      .then(response => {
        if(response.ok) {
          response.json().then(data => {
            this.news = data  // data.data.list
          })
        }
      })
      .catch(error => console.log(error))
    }
  },
  mounted() {
    this.getNewsList()
  }
}
</script>

<style>
html {
  font-family:
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

body {
  overflow: hidden;
  height: 100%;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}

#header {
  position: fixed;
  z-index: 99999;
  width: 50%;
  top: 0;
  left: 0;
  padding: 10px 40px 10px 20px;
}

#footer {
  position: fixed;
  z-index: -1;
  height: 50px;
  bottom: 10px;
  left: 0;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  width: 50%;
}
#content_right {
  width: 50%;
  background: #333;
  padding: 10vh 10vw;
  height: 100vh;
}
#content_left {
  width: 50%;
  padding: 10vh 10vh;
  padding-bottom: 50px;
  height: 100vh;
}
#content_left > div {
  margin-bottom: 20px;
}

#content_right > div {
  overflow: hidden;
  height: 100%;
}

#news {
  position: relative;
  left: 0px;
  width: calc(100% + 20px);
  height: 100%;
  overflow-y: scroll;
}

#news >>> .ant-card, #news >>> .ant-card-head {
  background: unset;
  color: #fff;
  border: unset;
}
</style>
