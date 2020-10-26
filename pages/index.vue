<template>
  <div style="display: flex">
    <div id="news">
      <div v-for="(item, index) in news" :key="index">
        <a-card style="margin-bottom:20px;" :loading="loading" :title="item.title">{{ item.message }}</a-card>
      </div>
    </div>
    <div id="content_right">
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
  </div>
</template>
<script>
export default {
  layout: 'index',
  data() {
    return {
      loading: false,
      todoList: [],
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
  },
};
</script>

<style scoped>
.ant-page-header.has-breadcrumb {
  background-color: #ffffffe0;
}
#news {
  width: 49%;
  margin-right: 1%;
  padding: 4%;
  background: url('/wallpaper-1.jpg') no-repeat center center;
}
#content_right {
  width: 49%;
  margin-left: 1%;
}
#content_right > div {
  margin-bottom: 20px;
}
</style>