# 我的个人博客（Jekyll + GitHub Pages）

站点地址：[https://sailorlab.github.io/blog/](https://sailorlab.github.io/blog/)

## 📖 站点说明

基于Jekyll搭建的静态博客，部署在GitHub Pages。
文章存放目录：`_posts/`
图片资源目录：`assets/images/`
页面模板目录：根目录、`_layouts/`

> 
> 全局样式约定：`.container { max-width: 1100px; }`；文章列表统一两列 Grid 网格布局，支持加载更多；文章缩略图宽度 300px。

## ✅ 页面清单

| 页面文件 | 页面标题 | 访问路径(permalink) | 功能说明 | 状态 |
| --- | --- | --- | --- | --- |
| `index.md` | 首页 | `/` | 网站首页，文章列表两列网格、加载更多；顶部标题「破晓 PO.XIAO」打字机动画；轮播图带实时时间 + 文字打字效果 | ✅ 完成 |
| `archives.md` | 文章归档 | `/archives/` | 全部文章归档列表，按年份分组展示；两列网格卡片，和首页样式统一 | ✅ 完成 |
| `tags.md` | 标签页 | `/tags/` | 标签云，点击标签筛选对应文章；文章列表两列网格，与首页布局对齐 | ✅ 完成 |
| `categories/life.md` | 关于生活 | `/categories/life/` | Life分类文章列表，两列网格；缩略图加宽定制 | ✅ 完成 |
| `categories/work.md` | 工作及其他 | `/categories/work/` | Work分类文章列表，两列网格，样式与首页统一 | ✅ 完成 |
| `categories/hobby.md` | 一点兴趣 | `/categories/hobby/` | Hobby分类文章列表，两列网格，样式与首页统一 | ✅ 完成 |
| `about.md` | 关于页面 | `/about/` | 个人介绍页面 | ✅ 完成 |

## 📌 页面样式统一规范

1. 容器：`.container` 固定最大宽度 `1100px`，左右自动居中，所有页面内容宽度统一，和顶部轮播左右对齐
2. 文章列表：CSS Grid `repeat(2,1fr)` 两列布局；移动端自动变为单列
3. 文章卡片：`post-card`，图文并排；缩略图默认宽度 `300px`，移动端自适应100%宽度
4. 动画：首页标题打字机（200ms/字符，打完停留5s循环，带下划线闪烁光标）；首页轮播自带实时时间更新 + 文字打字效果

## 📌 Jekyll 重大踩坑记录（重点！）

> 
> Bug现象：首页/分类列表点击文章404，浏览器地址缺少 `/blog` 前缀
> 正确地址：`[https://sailorlab.github.io/blog/2026/09/25/](https://sailorlab.github.io/blog/2026/09/25/)中秋/`
> 错误地址：`[https://sailorlab.github.io/2026/09/25/](https://sailorlab.github.io/2026/09/25/)中秋/`

### 问题根源

分类页面最初使用 `layout: page`，独立page布局渲染环境异常，`{{ post.url }}` 不会自动拼接 `site.baseurl`（`/blog`）。
首页虽然用`layout: default`，但`{{ post.url }}`同样存在丢失baseurl风险，因此全站列表链接统一兜底加固。
同时 `layout: page` 不会加载 `default.html` 的顶部导航栏，页面缺少头像菜单。

### ✅ 强制编码规范（新增页面必须遵守）

1. **所有页面 Front Matter，布局必须写 `layout: default`，禁止使用 `layout: page`**
静态页面（about）、列表页面（首页/归档/分类/标签）全部统一。
2. **所有文章列表的跳转链接，固定使用兜底写法：**

```
<a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
```

## 📋 待做事项

- 将页面链接添加至 Header 顶部导航
- 将页面链接添加至页脚导航
- 本地启动 Jekyll，逐页面预览校验：布局、加载更多、标签筛选交互
- 按需微调卡片间距、字体大小、缩略图尺寸

## 🔧 调试提示

页面出现布局错乱、加载更多失效等问题，直接提交对应页面源码用于排查。

直接复制全部内容粘贴进你的 `README.md` 即可。
如果你需要，我可以再加一段本地运行jekyll的启动命令。

## 🖼️ 图片资源维护注意事项（GitHub仓库限制）
> 本博客图片存放路径：`assets/images/`

### GitHub 仓库限制
1. 仓库整体**建议不要超过 1GB**；单文件大于 50MB 推送会警告，**大于100MB禁止提交**。
2. Git 会保存全部提交历史：图片即使后续删除，旧版本依旧占用仓库空间，不要反复修改、重复上传同一张图片。
3. GitHub Pages 每月出站流量软限制约100GB，个人博客正常访问基本不会触发限流。
4. **不建议使用 Git LFS 存放博客配图**：免费LFS有下载带宽配额，访问量大时图片会加载失败。

### ✅ 图片上传规范
1. 上传前本地压缩图片：JPG控制在 `200‑600KB`，网页展示宽度建议 `1200‑1600px`，不要直接上传手机直出数MB的原图。
2. 视频资源**禁止提交进本仓库**，优先使用外部视频平台嵌入。

### 📈 图片数量变多之后的备选方案
当仓库体积接近 800MB，建议把大量配图迁移至外部免费对象存储（如 Cloudflare R2），仓库内仅保留文章封面缩略小图，文章使用外部图片链接，避免仓库体积超限。

### 📊 查看仓库占用大小（新版GitHub不再在仓库设置页直接显示）
1. API查询（浏览器打开）：`https://api.github.com/repos/sailorlab/blog`，`size`字段单位为KB，除以1024得到MB；数据存在数小时延迟。
2. 本地仓库终端执行命令（最准确）
```bash
git gc
git count-objects -vH

GitHub仓库主页 → Settings → General → 页面底部查看仓库存储占用。
