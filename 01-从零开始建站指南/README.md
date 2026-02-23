# 🚀 从零开始：免费搭建个人博客完整指南

> 用 Node.js + Hexo + GitHub Pages，无需服务器，无需域名费用
> 适合完全零基础的新手，手把手教学

------

## 📋 一、准备工作（15分钟）

### 1.1 注册 GitHub 账号

1. 访问 [GitHub官网](https://github.com/)
2. 点击 **Sign up**，输入用户名（如 `yourname`）、邮箱、密码
3. 验证邮箱后完成注册

### 1.2 安装必要软件

| 软件        | 作用           | 下载地址                            | 安装说明                    |
| ----------- | -------------- | ----------------------------------- | --------------------------- |
| **Node.js** | Hexo 运行环境  | [nodejs.org](https://nodejs.org/)   | 下载 LTS 版本，一路“下一步” |
| **Git**     | 版本控制和部署 | [git-scm.com](https://git-scm.com/) | 下载 Windows 版本，默认安装 |

### 1.3 验证安装

打开 **Git Bash**（安装 Git 后会在右键菜单出现），输入：

```
node -v     # 显示版本号如 v18.18.0 即成功
git --version  # 显示版本号如 git 2.42.0 即成功
```

------

## 🏗️ 二、本地搭建博客（20分钟）

### 2.1 安装 Hexo

```
npm install -g hexo-cli
```

### 2.2 初始化博客

```
# 创建博客文件夹（名字自取）
hexo init myblog

# 进入博客目录
cd myblog

# 安装依赖
npm install
```

### 2.3 本地预览

```
hexo g      # 生成静态文件
hexo s      # 启动本地服务器
```

打开浏览器访问 `http://localhost:4000`，看到博客首页即成功
按 `Ctrl + C` 可停止预览

------

## 🎨 三、更换漂亮主题（10分钟）

以广受欢迎的 **NexT 主题** 为例：

### 3.1 下载主题

```
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

### 3.2 启用主题

编辑博客根目录的 `_config.yml` 文件：

```
theme: next
```

### 3.3 应用主题

```
hexo clean && hexo g && hexo s
```

刷新 `http://localhost:4000` 查看新主题

------

## ☁️ 四、部署到 GitHub Pages（15分钟）

### 4.1 创建 GitHub 仓库

1. 登录 GitHub，点击右上角 **+** → **New repository**
2. **仓库名必须填**：`你的用户名.github.io`
   （例如用户名为 `yourname`，就填 `yourname.github.io`）
3. 选择 **Public**，点击 **Create repository**

### 4.2 安装部署插件

```
npm install hexo-deployer-git --save
```

### 4.3 配置部署信息

编辑博客根目录 `_config.yml`，修改 deploy 部分：

```
deploy:
  type: git
  repo: https://github.com/你的用户名/你的用户名.github.io.git
  branch: main  # 或 master，看你的仓库默认分支
```

### 4.4 生成并部署

```
hexo clean && hexo g -d
```

### 4.5 访问博客

```
https://你的用户名.github.io
```

等待 2-5 分钟，就能看到博客上线了！🎉

------

## 🌐 五、绑定自定义域名（可选）

### 5.1 注册免费域名（以 DNSHe 为例）

1. 访问 [DNSHe](https://my.dnshe.com/) 注册账号
2. 选择免费后缀如 `bbroot.com`、`de5.net`
3. 填写前缀，如 `myblog.bbroot.com`
4. 完成注册

### 5.2 添加 DNS 记录

在 DNSHe 后台添加 4 条 A 记录：

| 记录类型 | 主机名 | 记录值          |
| -------- | ------ | --------------- |
| A        | @      | 185.199.108.153 |
| A        | @      | 185.199.109.153 |
| A        | @      | 185.199.110.153 |
| A        | @      | 185.199.111.153 |

（可选）添加 CNAME 让 `www` 也能访问：

| 记录类型 | 主机名 | 记录值                 |
| -------- | ------ | ---------------------- |
| CNAME    | www    | `你的用户名.github.io` |

### 5.3 在 GitHub 设置域名

1. 进入仓库 **Settings → Pages**
2. **Custom domain** 输入你的域名（如 `myblog.bbroot.com`）
3. 点击 **Save**
4. 勾选 **Enforce HTTPS**（等几分钟后出现）

### 5.4 创建 CNAME 文件（防止域名被清空）

```
cd myblog
echo "你的域名" > source/CNAME
git add .
git commit -m "添加 CNAME 文件"
git push
```

------

## ✍️ 六、日常写作与维护

### 6.1 写新文章

```
cd myblog
hexo new "文章标题"
```

编辑 `source/_posts/文章标题.md`

### 6.2 部署更新

```
hexo clean && hexo g -d
```

### 6.3 常用命令速查

| 命令                   | 作用         |
| ---------------------- | ------------ |
| `hexo s`               | 本地预览     |
| `hexo clean`           | 清理缓存     |
| `hexo g`               | 生成静态文件 |
| `hexo d`               | 部署上线     |
| `hexo new "标题"`      | 新建文章     |
| `hexo new page "名称"` | 新建独立页面 |

------

## 🧩 七、常见问题解决

### Q1：部署后页面还是旧的

- 等待 2-5 分钟
- 按 `Ctrl + F5` 强制刷新
- 检查 Actions 页面是否构建成功

### Q2：自定义域名被清空

- 检查 `source/CNAME` 文件是否存在
- 文件内容应为 `你的域名`（不要加 http://）

### Q3：`git push` 提示权限错误

- 使用 Personal Access Token 代替密码
- 或配置 SSH 密钥（参考网上教程）

### Q4：本地预览正常，线上 404

- 检查 GitHub Pages 是否开启（Settings → Pages）
- 确认分支选择正确（master 或 main）
- 检查 `.nojekyll` 文件是否存在

------

## 🔐 八、注意事项

1. **配置文件格式**：`_config.yml` 里冒号后面必须加空格
2. **不要提交敏感信息**：API 密钥、token 不要上传到 GitHub
3. **定期备份**：`source/` 和 `_config.yml` 是最重要的文件
4. **域名续期**：免费域名每年需手动续期（或设置自动脚本）

------

## 📚 九、进阶学习资源

- [Hexo 官方文档](https://hexo.io/zh-cn/docs/)
- [NexT 主题文档](https://theme-next.js.org/)
- [GitHub Pages 官方指南](https://pages.github.com/)
- [Markdown 语法入门](https://markdown.com.cn/)

------

## 🎉 十、恭喜！

按照本指南，你已经拥有：

- ✅ 一个完全属于自己的技术博客
- ✅ 免费托管在 GitHub Pages
- ✅ 可绑定自定义域名
- ✅ 支持 HTTPS 安全访问
- ✅ 完整的写作发布流程

**现在开始写你的第一篇文章吧！** 🚀

