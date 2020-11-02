<template>
  <div>

    <div id="content_left">
      <div id="todo_list">
        <nuxt-link to="/todo">
          <a-card :loading="loading" title="待办">
            <a-card-grid style="text-align:center;overflow: hidden;" v-for="(item, index) in todoList" :key="index" :hoverable="true">
              {{ item }}
            </a-card-grid>
          </a-card>
        </nuxt-link>
      </div>
      <div id="bookmarks">
        <nuxt-link to="/bookmarks">
          <a-card :loading="loading" title="书签">
            <a-card-grid style="text-align:center;overflow: hidden;" v-for="(item, index) in todoList" :key="index" :hoverable="true">
              {{ item }}
            </a-card-grid>
          </a-card>
        </nuxt-link>
      </div>
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

  </div>
</template>
<script>
export default {
  layout: 'index',
  data() {
    return {
      loading: false,
      todoList: [],
      news: [],
    };
  },
  head: {
    title: "青雲學府：三更燈火五更雞，正是男兒讀書時。黑髮不知勤學早，白首方悔讀書遲。",
    meta: [
      {
        hid: "description",
        name: "description",
        content:
          "任何人的内心都有幽暗的成分，所以人组成的任何机构天然都有幽暗的可能，一次犯罪不过是污染了水流，可一次不公正的司法却是污染了水源。问渠那得清如许，为有源头活水来。",
      },
    ],
  },
  mounted() {
    this.getNewsList()
    this.$nextTick(() => {
      this.$nuxt.$loading.start();
      setTimeout(() => this.$nuxt.$loading.finish(), 2000);
    });
    if(localStorage.getItem("textTodoList") == '[]' || localStorage.getItem("textTodoList") == null) {
      return;
    }
    this.todoList= JSON.parse(localStorage.getItem("textTodoList"));
  },
  methods: {
    onChange(a, b, c) {
      console.log(a, b, c);
    },
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
};
</script>

<style scoped>
.ant-page-header.has-breadcrumb {
  background-color: #ffffffe0;
}
#content_right {
  width: 50%;
  background: #333;
  padding: 10vh 10vw;
}
#content_left {
  width: 50%;
  padding: 10vh 10vw;
  padding-bottom: 50px;
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
  padding: 4%;
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