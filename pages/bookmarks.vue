<template>
  <div>
    <a-input style="margin-bottom:10px;" v-model="bookmarkInfo.title" placeholder="输入书签名称" @keydown.enter="enterToAddBookmark" />
    <a-input v-model="bookmarkInfo.link" placeholder="复制链接回车" @keydown.enter="enterToAddBookmark" />

    <a-list style="background-color: #fff;margin-top:10px" bordered :data-source="links">
      <a-list-item slot="renderItem" slot-scope="item, index" :key="index">
        <a-checkbox @change="onChange">
          <a-input
            :value="item.title"
            placeholder="输入修改的标题"
            :max-length="25"
            style="width: 120px"
            @blur="changeBookmarkInfo($event, index)"
          />
        </a-checkbox>
        <a 
          style="color: var(--primary-color)"
          :href="item.link"
          target="_blank"
        >去查看</a>
        <!-- <a-time-picker :default-value="moment('12:08', 'HH:mm')" format="HH:mm" /> -->
        <a-icon style="margin-left:10px;" @click="deleteBookmarkItem(index)" type="delete" />
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
      bookmarkInfo: {
        title: '',
        link: '',
      },
      loading: false,
      links: [],
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
    title: "书签 - 存世留千古，晚恨从容破",
    meta: [
      {
        hid: "description",
        name: "description",
        content:
          "存世留千古，晚恨从容破",
      },
    ],
  },
  mounted() {
    this.$nextTick(() => {
      this.$nuxt.$loading.start();
      setTimeout(() => this.$nuxt.$loading.finish(), 2000);
    });

    if(localStorage.getItem("textBookmark") == '[]' || localStorage.getItem("textBookmark") == null) {
      return;
    }
    this.links= JSON.parse(localStorage.getItem("textBookmark"));
  },
  methods: {
    // moment,
    onChange(a, b, c) {
      console.log(a, b, c);
    },
    enterToAddBookmark (ev) {
        if (ev.keyCode === 13 && this.bookmarkInfo.title != '' && this.bookmarkInfo.link != '') {
          this.links.push(this.bookmarkInfo);
          localStorage.setItem("textBookmark", JSON.stringify(this.links));
          this.bookmarkInfo.title = '';
          this.bookmarkInfo.link = '';
          this.$message.success('添加成功');
        } else {
          this.$message.warning('请输入标题及链接回车');
        }
    },
    deleteBookmarkItem(index) {
      this.links.splice(index, 1)
      localStorage.setItem("textBookmark", JSON.stringify(this.links));
      this.$message.success('删除成功');
    },
    changeBookmarkInfo(e, index) {
      this.links[index].title = e.target.value
      localStorage.setItem("textBookmark", JSON.stringify(this.links));
      this.$message.success('修改成功');
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