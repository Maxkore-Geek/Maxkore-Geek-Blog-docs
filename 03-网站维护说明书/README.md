# 🌐 Maxkore 博客网站维护说明书

> 一份全面、易懂的 Hexo + GitHub Pages 博客维护指南
> 适用于日常写作、站点配置、故障排查与内容管理

------

## 📁 一、博客目录结构

```
MyBlog/
├── _config.yml          # 博客主配置文件（重要！）
├── package.json          # 依赖包管理
├── scaffolds/            # 文章模板（post/page/draft）
├── source/               # 内容源文件（核心！）
│   ├── _posts/           # 文章存放处（Markdown）
│   ├── 自定义页面/        # 如 CTMIF/、极客索引.html
│   └── CNAME              # 自定义域名文件
├── themes/                # 主题文件夹（如 next）
│   └── next/              # NexT 主题配置
├── public/                 # 生成的静态文件（不用管）
├── .deploy_git/            # 部署临时文件（不用管）
└── .gitignore              # Git 忽略规则
```

------

## ✍️ 二、日常写作与发布

### 1. 创建新文章

```
cd /e/MyBlog
hexo new "文章标题"
```

> 会在 `source/_posts/` 生成 `.md` 文件

### 2. 编辑文章

- 用 Typora、VS Code 或记事本打开

- 文件开头格式示例：

  ```
  title: 文章标题
  date: 2026-02-23 13:26:02
  tags: [标签1, 标签2]
  ```

### 3. 本地预览

```
hexo server
```

访问 `http://localhost:4000` 查看效果
按 `Ctrl + C` 停止预览

### 4. 部署上线

```
hexo clean && hexo g -d
```

> 清理缓存 → 生成静态文件 → 推送到 GitHub

### 5. 删除文章

```
rm source/_posts/要删除的文章.md
hexo clean && hexo g -d
```

------

## 📄 三、自定义页面（HTML / 独立页面）

### 1. 创建独立页面（如“关于”）

```
hexo new page about
```

生成 `source/about/index.md`，访问 `https://maxkore.bbroot.com/about/`

### 2. 创建自定义 HTML 文件

直接在 `source/` 下创建 `.html` 文件：

```
touch "/e/MyBlog/source/极客索引.html"
```

写入 HTML 内容后部署：

```
hexo clean && hexo g -d
```

访问 `https://maxkore.bbroot.com/极客索引.html`

------

## 🎨 四、主题与样式配置

### NexT 主题常用配置（`themes/next/_config.yml`）

```
# 菜单
menu:
  home: / || fa fa-home
  archives: /archives/ || fa fa-archive
  tags: /tags/ || fa fa-tags

# 头像
avatar:
  url: /images/avatar.gif

# 社交链接
social:
  GitHub: https://github.com/Maxkore-Geek || fab fa-github
```

### 主配置文件（`_config.yml`）

```
title: Maxkore的博客
subtitle: 代码之外，思考之上。
author: Maxkore
language: zh-CN
timezone: Asia/Shanghai

# 部署配置
deploy:
  type: git
  repo: git@github.com:Maxkore-Geek/Maxkore-Geek.github.io.git
  branch: master
```

------

## 🔧 五、常见故障排查

| 现象                   | 可能原因              | 解决方法                      |
| ---------------------- | --------------------- | ----------------------------- |
| `hexo g` 报 YAML 错误  | 文章开头格式不对      | 检查 `:` 后面是否有空格       |
| 部署后页面未更新       | GitHub Pages 缓存     | 等 2-5 分钟，`Ctrl + F5` 刷新 |
| `git push` 被拒绝      | 远程有更新未拉取      | `git pull` → `git push`       |
| 自定义域名 404         | DNS 未生效 / 未配置   | 检查 Settings → Pages 设置    |
| 文件推送成功但访问 404 | 文件名打错 / 路径不对 | 核对 URL 大小写和文件名       |

------

## 🔐 六、Git 与 GitHub 维护

### 日常同步

```
git status          # 查看当前状态
git add .           # 添加所有更改
git commit -m "说明" # 提交
git push            # 推送到 GitHub
```

### 分支与远程

```
git branch          # 查看当前分支
git remote -v       # 查看远程仓库地址
```

### 强制推送（仅在必要时）

```
git push -f origin master
```

------

## 🔄 七、域名与 HTTPS

### 当前域名

- 主域名：`maxkore.bbroot.com`
- 备用域名：`maxkore-geek.github.io`

### GitHub Pages 设置

- 仓库 → Settings → Pages
- Branch: `master` → `/ (root)`
- Custom domain: `maxkore.bbroot.com`
- ✅ Enforce HTTPS 已启用

### DNS 记录（DNSHe 后台）

| 类型  | 主机名 | 记录值                   |
| ----- | ------ | ------------------------ |
| A     | @      | 185.199.108.153          |
| A     | @      | 185.199.109.153          |
| A     | @      | 185.199.110.153          |
| A     | @      | 185.199.111.153          |
| CNAME | www    | `maxkore-geek.github.io` |

------

## ⏱️ 八、自动续期任务（Windows 计划）

### 脚本位置

```
E:\服务器(勿删)\renew.py
```

### 任务计划设置

- 名称：`DNSHE 域名自动续期`
- 触发器：每月 1 日 09:00
- 操作：`python "E:\服务器(勿删)\renew.py"`

### 日志查看

```
E:\服务器(勿删)\renew.log
```

------

## 📌 九、日常写作快速参考

### 一条命令搞定

```
cd /e/MyBlog && hexo new "新文章" && notepad source/_posts/新文章.md
```

写完保存后：

```
hexo clean && hexo g -d
```

### 常用命令速查

| 命令                   | 作用         |
| ---------------------- | ------------ |
| `hexo s`               | 本地预览     |
| `hexo clean`           | 清理缓存     |
| `hexo g`               | 生成静态文件 |
| `hexo d`               | 部署上线     |
| `hexo new "标题"`      | 新建文章     |
| `hexo new page "名称"` | 新建独立页面 |

------

## 🧠 十、经验总结

1. **写文章永远在 `source/_posts/`**
2. **改配置永远在 `_config.yml` 或主题的 `_config.yml`**
3. **部署永远用 `hexo clean && hexo g -d`**
4. **404 先等 2 分钟，再 `Ctrl + F5`**
5. **Git 冲突先 `git pull`，再 `git push`**
6. **网络问题重试几次，或换热点**

------

> 维护人：Maxkore
> 博客地址：[https://maxkore.bbroot.com](https://maxkore.bbroot.com/)