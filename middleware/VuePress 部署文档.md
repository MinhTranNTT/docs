### vuepress 部署文档

```sh
yarn create vuepress-theme-hope vuepress.github.io

yarn run docs:dev
```

### 参考文档：

[VuePress Vue 驱动的静态网站生成器](https://v2.vuepress.vuejs.org/zh/)

[vuepress-theme-hope 一个具有强大功能的 vuepress 主题](https://theme-hope.vuejs.press/zh/cookbook/)

[vuepress-plugin-md-enhance 为 VuePress2 提供更多 Markdown 增强功能](https://plugin-md-enhance.vuejs.press/zh/)



### 常见问题：

[vuepress部署到githab样式不起作用](https://blog.csdn.net/qq_30351747/article/details/130527623)

github 自动部署：

```yml
name: 部署文档
on:
push:
    branches:
    - main
jobs:
deploy-gh-pages:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
        uses: actions/checkout@v3
        with:
        fetch-depth: 0
    - name: 设置 Node.js
        uses: actions/setup-node@v3
        with:
        node-version: 18
        cache: yarn
    - name: 安装依赖
        run: yarn install --frozen-lockfile
    - name: 构建文档
        env:
        NODE_OPTIONS: --max_old_space_size=8192
        run: |-
        yarn run docs:build
    - name: 部署文档
        uses: JamesIves/github-pages-deploy-action@v4
        with:
        branch: gh-pages	# 这是文档部署到的分支名称
        folder: src/.vuepress/dist
        env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```