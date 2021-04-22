<template>
  <a-layout id="components-layout-demo-top-side-2">
    <div class="signup" v-if="false">
      <a-form-model layout="horizontal" :model="formInline" @submit="registerUser" @submit.native.prevent>
        <a-form-model-item>
          <a-input v-model="formInline.name" placeholder="Username">
            <a-icon slot="prefix" type="user" style="color:rgba(0,0,0,.25)" />
          </a-input>
        </a-form-model-item>
        <a-form-model-item>
          <a-input v-model="formInline.password" type="password" placeholder="Password">
            <a-icon slot="prefix" type="lock" style="color:rgba(0,0,0,.25)" />
          </a-input>
        </a-form-model-item>
        <a-form-model-item>
          <a-input v-model="formInline.phone" type="number" placeholder="Phone">
            <a-icon slot="prefix" type="phone" style="color:rgba(0,0,0,.25)" />
          </a-input>
        </a-form-model-item>
        <a-form-model-item>
          <a-input v-model="formInline.email" type="email" placeholder="Email">
            <a-icon slot="prefix" type="mail" style="color:rgba(0,0,0,.25)" />
          </a-input>
        </a-form-model-item>
        <a-form-model-item>
          <a-input v-model="formInline.introduce" type="textarea" placeholder="Introduce">
          </a-input>
        </a-form-model-item>
        <a-form-model-item>
          <a-button
            type="primary"
            html-type="submit"
            :disabled="formInline.user === '' || formInline.password === ''"
          >
            Register
          </a-button>
        </a-form-model-item>
      </a-form-model>

    </div>
  
    <a-layout-header class="header">
      <div class="logo" />
      <a-menu
        theme="dark"
        mode="horizontal"
        :default-selected-keys="['2']"
        :style="{ lineHeight: '64px' }"
      >
        <a-menu-item key="1">
          文章
        </a-menu-item>
      </a-menu>
    </a-layout-header>
    <a-layout>
      <a-layout-sider width="200" style="background: #fff">
        <a-menu
          mode="inline"
          :default-selected-keys="[$nuxt.$route.name]"
          :default-open-keys="[$nuxt.$route.name]"
          :style="{ height: '100%', borderRight: 0 }"
        >
          <a-sub-menu key="wakagi-posts">
            <span slot="title"><a-icon type="user" />文章</span>
            <a-menu-item key="wakagi-posts">
              <NuxtLink to="/wakagi/posts">列表</NuxtLink>
            </a-menu-item>
            <a-menu-item key="wakagi-posts-add">
              <NuxtLink to="/wakagi/posts/add">发布</NuxtLink>
            </a-menu-item>
          </a-sub-menu>
        </a-menu>
      </a-layout-sider>
      <a-layout style="padding: 0 24px 24px">
        <a-breadcrumb style="margin: 16px 0">
          <a-breadcrumb-item>
              <NuxtLink to="/wakagi">家</NuxtLink>
          </a-breadcrumb-item>
          <a-breadcrumb-item>
              <NuxtLink to="/wakagi/posts">文章</NuxtLink>
          </a-breadcrumb-item>
          <a-breadcrumb-item>
              <NuxtLink to="/wakagi/posts/add">发布</NuxtLink>
          </a-breadcrumb-item>
        </a-breadcrumb>
        <a-layout-content
          :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }"
        >
          <Nuxt id="main" />
        </a-layout-content>
      </a-layout>
    </a-layout>
  </a-layout>
</template>

<script>
export default {
  data() {
    return {
      code: '',
      formInline: {
        name: '',
        password: '',
        phone: '',
        email: '',
        introduce: '',
        type: '0'
      },
    }
  },
  methods: {
    handleSubmit(e) {
      console.log(this.formInline);
    },
    currentUser() {
      fetch('http://127.0.0.1:3000/currentUser', {
        method: 'GET',
      })
      .then(res => {
        if(res.ok) {
          res.json().then(data => {
            console.log(data);
          })
        }
      })
    },
    loginUser() {
      fetch('http://127.0.0.1:3000/loginAdmin', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          email: '15055114708@163.com',
          password: '159357wyq'
        })
      })
      .then(res => {
        console.log(res);
        if(res.ok) {
          res.json().then(data => {
            console.log(data);
            this.currentUser()
          })
        } else if (res.status == 400) {
          res.json().then(data => {
            this.$notification['error']({
              message: '登录失败',
              description:
                data.message,
            });
          })
        }
      })
    },
    registerUser() {
      fetch('http://127.0.0.1:3000/register', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.formInline),
      })
      .then(res => {
        if(res.ok) {
          res.json().then(data => {
            if (data.code == 0) {
              this.$notification['success']({
                message: data.message,
                duration: 0,
                description:
                  `您的用户信息：${JSON.stringify(this.formInline)}`,
              });
            }
          })
        } else if (res.status == 400) {
          res.json().then(data => {
            this.$notification['error']({
              message: '注册失败',
              description:
                data.message,
            });
          })
        }
      })
      .catch(error => console.log(error))
    }
  },
  mounted() {
    // this.loginUser()
    !localStorage.getItem("code") ? localStorage.getItem("code") : ''
  },
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

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

.signup {
  position: fixed;
  background-color: #fff;
  width: 100%;
  height: 100%;
  padding: 0 25%;
  left: 0;
  top: 0;
  align-items: center;
  justify-content: center;
  display: flex;
}
.signup > .ant-form {
  width: 100%;
}
</style>
