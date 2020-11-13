<template>
  <div>
    <a-input v-model="textTodo" placeholder="输入内容回车" @keydown.enter="enterToAddTodoList" />

    <a-list style="background-color: #fff;margin-top:10px" bordered :data-source="news">
      <a-list-item slot="renderItem" slot-scope="item, index" :key="index">
        <a-checkbox @change="onChange">
          <a-input
            :value="item"
            placeholder="输入修改的标题"
            :max-length="25"
            style="width: 120px"
            @blur="changeItemText($event, index)"
          />
        </a-checkbox>
        <!-- <a-time-picker :default-value="moment('12:08', 'HH:mm')" format="HH:mm" /> -->
        <a-icon style="margin-left:10px;" @click="deleteTodoItem(index)" type="delete" />
      </a-list-item>
      <div slot="footer">
        反馈
      </div>
    </a-list>
    <div >
    </div>
  </div>
</template>
<script>
// import moment from 'moment';

export default {
  layout: 'index',
  data() {
    return {
      textTodo: '',
      loading: false,
      news: [],
      options: {
        dir: "auto", // 文字方向
        body: "通知：OBKoro1评论了你的朋友圈", // 通知主体
        requireInteraction: true, // 不自动关闭通知
        // 通知图标 
        icon: "https://upload-images.jianshu.io/upload_images/5245297-818e624b75271127.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"
      }
    };
  },
  head: {
    title: "待办 - 行事含条理，千军万马从中过",
    meta: [
      {
        hid: "description",
        name: "description",
        content:
          "行事含条理，千军万马从中过",
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
    this.news= JSON.parse(localStorage.getItem("textTodoList"));
  },
  methods: {
    // moment,
    onChange(a, b, c) {
      console.log(a, b, c);
    },
    enterToAddTodoList (ev) {
        if (ev.keyCode === 13) {
          this.news.push(this.textTodo);
          localStorage.setItem("textTodoList", JSON.stringify(this.news));
          this.textTodo = '';
        }
    },
    deleteTodoItem(index) {
      this.news.splice(index, 1)
      localStorage.setItem("textTodoList", JSON.stringify(this.news));
    },
    changeItemText(e, index) {
      this.news[index] = e.target.value
      localStorage.setItem("textTodoList", JSON.stringify(this.news));
    },
    notifyMe(title, options) {
      // 先检查浏览器是否支持
      if (!window.Notification) {
        console.log('浏览器不支持通知');
      } else {
        // 检查用户曾经是否同意接受通知
        if (Notification.permission === 'granted') {
          var notification = new Notification(title, options); // 显示通知
        } else if (Notification.permission === 'default') {
          // 用户还未选择，可以询问用户是否同意发送通知
          Notification.requestPermission().then(permission => {
            if (permission === 'granted') {
              console.log('用户同意授权');
              var notification = new Notification(title, options); // 显示通知
            } else if (permission === 'default') {
              console.warn('用户关闭授权 未刷新页面之前 可以再次请求授权');
            } else {
              // denied
              console.log('用户拒绝授权 不能显示通知');
            }
          });
        } else {
          // denied 用户拒绝
          console.log('用户曾经拒绝显示通知');
        }
      }
    }
  },
};
</script>

<style scoped>
.ant-page-header.has-breadcrumb {
  background-color: #ffffffe0;
}
</style>