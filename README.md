# Nuxt-Blog-Front

1. ant-design-vue
2. Vue
3. Nuxt
4. ......

## Build Setup

```bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev

# build for production and launch server
$ yarn build
$ yarn start

# generate static project
$ yarn generate
```

## 增加 PWA 可安装功能

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).

## Github走本地VPN

开启连接到本地的8889端口
git config --global http.proxy http://127.0.0.1:8889
git config --global https.proxy http://127.0.0.1:8889
取消
git config --global --unset http.proxy
git config --global --unset https.proxy
