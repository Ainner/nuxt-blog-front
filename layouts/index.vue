<template>
  <section id="content" style="display:flex">
    <TheHeader id="header" />
    <div id="content_left">
      <div>
        <Nuxt id="main" />
      </div>
    </div>
    <div v-if="isNotMobile" id="content_right">
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
      isNotMobile: true
    }
  },
  methods: {
    getNewsList() {
      fetch('https://blog.chimeiwangliang.cn/wp-json/wp/v2/posts?categories=18', {
        method: 'GET',
      })
      .then(response => {
        if(response.ok) {
          response.json().then(data => {
            console.log(data);
            this.news = data  // data.data.list
          })
        }
      })
      .catch(error => console.log(error))
    },
    _isMobile(){
      let flag = navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i)
      return flag;
    }
  },
  mounted() {
    this.getNewsList();
    if (this._isMobile()) {
      this.isNotMobile = false
    }else {
      this.isNotMobile = true
    }
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
  -ms-overflow-style: none;
}
* {
  -ms-overflow-style: none;
  overflow: -moz-scrollbars-none;
}
*::-webkit-scrollbar { width: 0 !important }

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
  z-index: 9;
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
  text-align: center;
  padding: 0 5vw;
}

/* 左右大框架 */
#content_left, #content_right {
  width: 50%;
  height: 100vh;
  padding: 8vh 5vw;
}
/* 内容外包，隐藏滚动栏作用 */
#content_left > div, #content_right > div {
  overflow: hidden;
  height: 100%;
}
/* 内容 */
#content_left > div > #main, #news {
  width: 100%;
  height: 100%;
  overflow-y: scroll;
  padding: 20px;
}

#content_right {
  background: #333;
}

#content_right img {
  max-width: 100%;
  height: auto;
}

#news .ant-card-head-title {
  font-weight: 600;
}
#news pre.code {
  background: #333;
  color: #F78C6C;
  padding: 20px;
  border-radius: 4px;
  box-shadow: 0 0 3px #000 inset;
}

@media only screen and (max-width: 1000px) {
  #content {
    flex-direction: column;
  }

  #header, #content_left, #content_right, #footer {
    width: 100%;
  }
  #content_left, #content_right {
    height: 100vh;
  }
  #header ul#custom_menu li {
    margin-left: 20px;
  }
}
</style>
