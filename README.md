# 我的个人博客（Jekyll + GitHub Pages）
站点地址：https://sailorlab.github.io/blog/

## 📖 站点说明
基于Jekyll搭建的静态博客，部署在GitHub Pages。
文章存放目录：`_posts/`
图片资源目录：`assets/images/`
页面模板目录：根目录、`_layouts/`

## ✅ 页面清单
- index.html：入口，引用 home.html
- home.html：首页文章列表
- archives.html：归档页
- tags.html：标签页
- categories/life.html：关于生活 分类页
- categories/work.html：工作及其他 分类页
- categories/hobby.html：一点兴趣 分类页
- about.html：关于页面

## 📌 Jekyll 重大踩坑记录（重点！）
> Bug现象：首页/分类列表点击文章404，浏览器地址缺少 `/blog` 前缀，
> 正确地址：`https://sailorlab.github.io/blog/2026/09/25/中秋/`
> 错误地址：`https://sailorlab.github.io/2026/09/25/中秋/`

### 问题根源
分类页面最初使用 `layout: page`，独立page布局渲染环境异常，`{{ post.url }}` 不会自动拼接 `site.baseurl`（`/blog`）。
首页虽然用`layout: default`，但`{{ post.url }}`同样存在丢失baseurl风险，因此全站列表链接统一兜底加固。
同时 `layout: page` 不会加载 `default.html` 的顶部导航栏，页面缺少头像菜单。

### ✅ 强制编码规范（新增页面必须遵守）
1. **所有页面 Front Matter，布局必须写 `layout: default`，禁止使用 `layout: page`**
   静态页面（about）、列表页面（首页/归档/分类/标签）全部统一。
2. **所有文章列表的跳转链接，固定使用兜底写法：**
```liquid
<a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
