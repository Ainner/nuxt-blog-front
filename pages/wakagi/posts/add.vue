<template>
  <div class="container">
    <div style="width: 28%;margin-right: 2%;">
      <a-form-model
          ref="ruleForm"
          :model="form"
          :rules="rules"
          layout="vertical"
        >
        <a-form-model-item ref="title" prop="title">
          <a-input addon-before="标题" v-model="form.title" />
        </a-form-model-item>

        <a-form-model-item ref="author" prop="author">
          <a-input addon-before="作者" v-model="form.author" />
        </a-form-model-item>

        <a-form-model-item ref="desc" prop="desc">
          <a-input addon-before="描述" v-model="form.desc" />
        </a-form-model-item>

        <a-select default-value="publish">
          <a-select-option value="publish">
            发布
          </a-select-option>
          <a-select-option value="draft">
            草稿
          </a-select-option>
        </a-select>

        <a-select placeholder="输入标签" mode="tags" style="width: 100%" :token-separators="[',']">
          <a-select-option v-for="i in 25" :key="(i + 9).toString(36) + i">
            {{ (i + 9).toString(36) + i }}
          </a-select-option>
        </a-select>

        <a-select placeholder="输入分类" mode="tags" style="width: 100%" :token-separators="[',']">
          <a-select-option v-for="i in 25" :key="(i + 9).toString(36) + i">
            {{ (i + 9).toString(36) + i }}
          </a-select-option>
        </a-select>

        <a-popconfirm placement="bottomLeft" ok-text="是" cancel-text="否" @confirm="confirm">
          <template slot="title">
            <p>{{ text }}</p>
          </template>
          <a-button type="primary" icon="edit" :loading="iconLoading">
            {{ pushButton }}
          </a-button>
        </a-popconfirm>

      </a-form-model>

    </div>

    <div style="width: 70%;">
      <div class="quill-editor"
        :content="form.content"
        @change="onEditorChange($event)"
        v-quill:myQuillEditor="editorOption">
      </div>
    </div>

  </div>
</template>
<script>

export default {
  layout: 'wakagi',
  data() {
    return {
      text: '是否已准备齐全？',
      pushButton: '发布',
      form: {
        title: '',
        author: 'wakagi',
        // keyword: [],
        content: '<p>I am Example</p>',
        desc: '',
        // tags: [],
        // category: ['undefind'],
        state: '1',
        type: '1',
        origin: '0',
      },
      rules: {
        title: [
          { required: true, message: '请输入文章标题', trigger: 'blur' },
          { min: 0, max: 50, message: '最高50个字符', trigger: 'blur' },
        ],
        author: [{ required: true, message: '请输入作者名称', trigger: 'blur' }],
        desc: [{ required: true, message: '请输入文章简介', trigger: 'blur' }],
      },
      iconLoading: false,
      editorOption: {
        // some quill options
        modules: {
          toolbar: [
            ['bold', 'italic', 'underline', 'strike'],
            ['blockquote', 'code-block'],
            [{ 'header': 1 }, { 'header': 2 }],
          ]
        }
      }
    };
  },
  head: {
    title: "新加文章",
    meta: [
      {
        hid: "description",
        name: "description",
        content:
          "新加文章",
      },
    ],
  },
  mounted() {
    console.log(this.$route.params);
    if(this.$route.params.id) {
      this.getArticleDetail()
      this.form._id = this.$route.params.id
    }
  },
  methods: {
    confirm() {
      this.onSubmit()
    },
    addArticle() {
      let url = this.pushButton == '更新' ? 'http://chimeiwangliang.cn/api/updateArticle' : 'http://chimeiwangliang.cn/api/addArticle'
      fetch(url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json' 
        },
        body: JSON.stringify(this.form)
      })
      .then(response => {
        if(response.ok) {
          response.json().then(data => {
            console.log(data);
            this.$message.success(data.message);
            this.iconLoading = false;
          })
        }
      })
      .catch(error => console.log(error))
    },
    getArticleDetail() {
      fetch('http://chimeiwangliang.cn/api/getArticleDetail', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json' 
        },
        body: JSON.stringify({
          id: this.$route.params.id,
          type: 1,
          filter: 2
        })
      })
      .then(response => {
        if(response.ok) {
          response.json().then(data => {
            console.log(data.data);
            for(let key in this.form) {
              this.form[key] = data.data[key]
            }
            this.pushButton = '更新'
            this.form.img_url = data.data.img_url
            this.$message.success(data.message);
          })
        }
      })
      .catch(error => console.log(error))
    },
    onSubmit() {
      if(!this.form.content) {
        this.iconLoading = false;
        return this.$message.error('请输入文章内容');
      }
      this.$refs.ruleForm.validate(valid => {
        if (valid) {
          // this.iconLoading = true;
          this.addArticle();
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    onEditorChange({ editor, html, text }) {
      this.form.content = html
    }
  },
};
</script>

<style scoped>
.container {
  display: flex;
}
.ant-form-vertical > * {
  margin: 0 0 8px !important;
  padding-bottom: 0;
}
</style>
