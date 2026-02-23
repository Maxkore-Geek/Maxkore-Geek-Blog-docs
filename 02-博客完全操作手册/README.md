# 📚 Hexo 博客完全操作手册 & 错误解决大全

------

## 📋 一、博客日常操作（标准流程）

### 1.1 写新文章

```
# 进入博客目录
cd /e/MyBlog

# 创建新文章（标题可以用中文）
hexo new "文章标题"

# 编辑文章（用 Typora/VS Code/记事本）
notepad source/_posts/文章标题.md
```

**文章头部格式示例**：

```
title: 文章标题
date: 2026-02-23 13:26:02
tags: [标签1, 标签2]
categories: [分类名]
```

### 1.2 ⚠️ 创建文章时的常见错误

#### 错误示例：命令中包含特殊符号

```
# ❌ 错误：& 符号导致命令中断
hexo new "Hexo 博客 & 错误解决大全" && notepad source/_posts/Hexo 博客 & 错误解决大全.md
bash: 错误解决大全.md: command not found
```

#### ✅ 正确方法 1：分两步执行（最稳妥）

```
# 第一步：创建文章
hexo new "文章标题"

# 第二步：打开编辑
notepad source/_posts/文章标题.md
```

#### ✅ 正确方法 2：用引号保护特殊字符

```
hexo new "Hexo 博客 & 错误解决大全" && notepad "source/_posts/Hexo 博客 & 错误解决大全.md"
```

#### ✅ 正确方法 3：文件名用短横线避免特殊字符

```
hexo new "hexo-complete-manual" && notepad source/_posts/hexo-complete-manual.md
```

#### 如果文章已创建但打不开

```
# 查看刚创建的文件
ls -l source/_posts/

# 直接用文件名打开（注意用引号）
notepad "source/_posts/文章标题.md"
```

### 1.3 本地预览

```
hexo server
# 或指定端口
hexo s -p 4001
```

访问 `http://localhost:4000` 查看效果
按 `Ctrl + C` 停止预览

### 1.4 部署上线

```
# 标准三步曲
hexo clean      # 清理缓存
hexo generate   # 生成静态文件（可简写为 hexo g）
hexo deploy     # 部署到 GitHub（可简写为 hexo d）

# 一键完成
hexo clean && hexo g -d
```

### 1.5 修改文章

```
# 直接编辑源文件
notepad source/_posts/文章标题.md

# 重新部署
hexo clean && hexo g -d
```

### 1.6 删除文章

```
# 删除源文件
rm source/_posts/要删除的文章.md

# 清理并重新部署
hexo clean && hexo g -d
```

### 1.7 创建独立页面

```
# 创建“关于”页面
hexo new page about

# 创建自定义 HTML 页面
touch source/自定义页面.html
```

------

## 🌐 二、自定义域名配置与修复

### 2.1 首次配置自定义域名

#### 在 GitHub 设置

1. 进入仓库 **Settings → Pages**
2. 在 **Custom domain** 输入你的域名（如 `maxkore.bbroot.com`）
3. 点击 **Save**
4. 勾选 **Enforce HTTPS**（等几分钟后出现）

#### 创建 CNAME 文件（防止域名被清空）

```
# 进入博客目录
cd /e/MyBlog

# 创建 CNAME 文件（注意：没有扩展名）
echo "你的域名" > source/CNAME

# 验证文件内容
cat source/CNAME
# 应该只显示你的域名，没有多余空格或换行
```

### 2.2 ⚠️ 域名消失问题修复（重要！）

**现象**：每次部署后，GitHub Pages 设置里的自定义域名输入框被清空

**原因**：`CNAME` 文件缺失、内容错误或未被正确提交

#### 修复步骤：

```
# 1. 检查本地 CNAME 文件
cd /e/MyBlog
cat source/CNAME
# 如果不存在或内容错误，重新创建

# 2. 强制创建正确的 CNAME 文件（-n 参数避免多余换行）
echo -n "maxkore.bbroot.com" > source/CNAME

# 3. 确认内容（应该只有域名，没有额外换行）
cat source/CNAME

# 4. 提交到 Git 仓库
git add source/CNAME
git commit -m "修复 CNAME 文件，确保域名永久生效"
git push

# 5. 重新部署博客
hexo clean && hexo g -d
```

#### 验证是否成功：

- 访问 GitHub 仓库：`https://github.com/你的用户名/你的用户名.github.io`
- 查看根目录是否有 **`CNAME`** 文件
- 点开文件，内容应为你的域名（如 `maxkore.bbroot.com`）
- 去 **Settings → Pages**，域名应该已经自动填好

### 2.3 域名消失的常见原因

| 可能原因               | 说明              | 解决方法                              |
| ---------------------- | ----------------- | ------------------------------------- |
| **CNAME 文件不存在**   | 忘记创建          | `echo "域名" > source/CNAME`          |
| **CNAME 文件内容错误** | 有多余空格或换行  | 用 `echo -n` 重新创建                 |
| **CNAME 文件没提交**   | 只创建没推送      | `git add` + `git commit` + `git push` |
| **文件在错误位置**     | 不在 `source/` 下 | 移到 `source/` 文件夹                 |
| **GitHub 缓存**        | 系统没及时更新    | 手动重新输入保存                      |

### 2.4 永久解决方案

```
# 创建完美的 CNAME 文件（一次搞定）
cd /e/MyBlog
echo -n "maxkore.bbroot.com" > source/CNAME

# 添加到 Git 并推送
git add source/CNAME
git commit -m "添加 CNAME 文件，确保自定义域名永久生效"
git push

# 以后每次部署都会自动保留域名
hexo clean && hexo g -d
```

------

## 🔧 三、Git 工作流与同步

### 3.1 日常 Git 操作

```
# 查看当前状态
git status

# 添加所有更改
git add .

# 提交更改
git commit -m "提交说明"

# 推送到 GitHub
git push

# 拉取远程更新
git pull
```

### 3.2 完整推送流程

```
git add .
git commit -m "更新说明"
git pull origin master
git push origin master
```

### 3.3 强制推送（仅在必要时）

```
git push -f origin master
```

------

## 🚨 四、错误解决大全

### 4.1 YAML 解析错误

**错误信息**：

```
YAMLException: can not read a block mapping entry
```

**原因**：文章开头的配置格式错误
**解决**：

- 检查 `:` 后面是否有空格

- 确保缩进正确

- 示例正确格式：

  ```
  title: 正确格式  # ✅ 冒号后有空格
  tags: [标签]     # ✅ 正确
  tags: [标签]     # ❌ 错误
  ```

### 4.2 部署失败 - 网络问题

**错误信息**：

```
fatal: unable to access '...': Recv failure: Connection was reset
```

**原因**：GitHub 网络波动
**解决**：

```
# 方法1：稍后重试
hexo clean && hexo g -d

# 方法2：手动 Git 推送
git add .
git commit -m "更新"
git push

# 方法3：切换网络（WiFi换热点）
```

### 4.3 Git 推送被拒绝

**错误信息**：

```
! [rejected] master -> master (fetch first)
```

**原因**：远程有本地没有的更新
**解决**：

```
git pull origin master
git push origin master
```

### 4.4 合并冲突

**错误信息**：

```
Automatic merge failed; fix conflicts and then commit the result.
```

**现象**：处于 `(master|MERGING)` 状态
**解决**：

```
# 1. 查看冲突文件
git status

# 2. 添加已解决的文件
git add .

# 3. 完成合并
git commit -m "解决合并冲突"

# 4. 推送
git push origin master
```

### 4.5 子模块错误

**错误信息**：

```
fatal: No url found for submodule path '.deploy_git' in .gitmodules
```

**原因**：Git 误将文件夹当作子模块
**解决**：

```
# 彻底清理
rm -rf .gitmodules
git rm --cached .deploy_git
git add .
git commit -m "移除子模块配置"
git push
```

### 4.6 SSH 密钥问题

**错误信息**：

```
git@github.com: Permission denied (publickey)
```

**原因**：SSH 密钥未配置或密码错误
**解决**：

```
# 生成新密钥（无密码）
ssh-keygen -t rsa -b 4096 -C "你的邮箱"
# 一直按回车（不设密码）

# 查看公钥
cat ~/.ssh/id_rsa.pub

# 复制公钥添加到 GitHub
# Settings → SSH and GPG keys → New SSH key
```

### 4.7 Token 认证问题

**错误信息**：

```
remote: Invalid username or token.
```

**原因**：Token 错误或过期
**解决**：

```
# 重新生成 Token 后使用
git remote set-url origin https://用户名:新token@github.com/用户名/仓库名.git
git push
```

### 4.8 本地预览端口被占用

**错误信息**：

```
Error: listen EADDRINUSE
```

**解决**：

```
# 换端口启动
hexo s -p 4001
```

### 4.9 文章不显示

**现象**：部署成功但文章没出现
**原因**：文章头部格式错误
**解决**：

- 检查 `date` 格式是否正确
- 确保 `title` 不为空
- 运行 `hexo clean` 后重新生成

------

## 📝 五、配置文件详解

### 5.1 主配置文件 `_config.yml`

```
# 网站信息
title: 博客标题
subtitle: 副标题
description: 描述
author: 作者名
language: zh-CN
timezone: Asia/Shanghai

# 部署配置
deploy:
  type: git
  repo: git@github.com:用户名/用户名.github.io.git
  branch: master
```

### 5.2 NexT 主题配置 `themes/next/_config.yml`

```
# 菜单
menu:
  home: / || fa fa-home
  archives: /archives/ || fa fa-archive
  tags: /tags/ || fa fa-tags
  about: /about/ || fa fa-user

# 头像
avatar:
  url: /images/avatar.gif

# 社交链接
social:
  GitHub: https://github.com/你的用户名 || fab fa-github
```

------

## 🧹 六、日常维护清单

### 6.1 每周任务

-  写新文章
-  本地预览确认格式
-  部署上线

### 6.2 每月任务

-  检查域名续期状态
-  更新依赖 `npm update`
-  备份 `source/` 和 `_config.yml`

### 6.3 域名维护（重要）

-  确认 `source/CNAME` 文件存在且内容正确
-  每月初检查 GitHub Pages 设置里自定义域名是否还在
-  如果消失，按 2.2 节步骤修复

------

## 📌 七、常用命令速查表

| 命令                            | 作用         | 场景        |
| ------------------------------- | ------------ | ----------- |
| `hexo new "标题"`               | 新建文章     | 写作        |
| `hexo s`                        | 本地预览     | 检查效果    |
| `hexo clean`                    | 清理缓存     | 解决异常    |
| `hexo g`                        | 生成静态文件 | 部署前      |
| `hexo d`                        | 部署         | 上线        |
| `hexo clean && hexo g -d`       | 一键部署     | 日常发布    |
| `git status`                    | 查看状态     | 检查更改    |
| `git add .`                     | 添加更改     | 提交前      |
| `git commit -m "说明"`          | 提交         | 本地记录    |
| `git push`                      | 推送         | 同步 GitHub |
| `git pull`                      | 拉取         | 获取更新    |
| `echo -n "域名" > source/CNAME` | 创建 CNAME   | 域名配置    |

------

## 💡 八、经验总结

### 8.1 黄金法则

1. **写文章前**：先 `git pull` 确保最新
2. **部署前**：先 `hexo clean` 避免缓存问题
3. **遇到错误**：先看错误日志，再搜解决方案
4. **重要操作前**：先备份 `source/` 文件夹
5. **域名配置**：永远在 `source/` 下放 `CNAME` 文件

### 8.2 创建文章注意事项

| 注意点                | 正确做法                                |
| --------------------- | --------------------------------------- |
| **标题包含 `&` 符号** | 用引号括起来，或分步执行                |
| **标题包含空格**      | 用引号括起来                            |
| **同时打开编辑器**    | 分两步执行：先 `hexo new`，再 `notepad` |
| **文件名过长**        | 用短横线连接，避免特殊字符              |

### 8.3 常见误区

- ❌ 直接在 GitHub 网页修改文件
- ❌ 忘记 `:` 后的空格
- ❌ 提交 `node_modules/` 到仓库
- ❌ 推送前不拉取远程更新
- ❌ **忘记检查 CNAME 文件**（导致域名消失）
- ❌ **命令中包含 `&` 等特殊字符不加处理**

### 8.4 最佳实践

- ✅ 每次写文章前 `git pull`
- ✅ 每次部署用 `hexo clean && hexo g -d`
- ✅ 定期备份 `source/` 文件夹
- ✅ 写提交信息时用中文说明
- ✅ **每月检查一次 CNAME 文件**（防患未然）
- ✅ **创建文章时分两步执行**（避免命令解析错误）

------

## 🎉 九、恭喜！

你现在拥有：

- ✅ 完整的博客操作知识
- ✅ 所有常见错误的解决方案
- ✅ **自定义域名永久保留的秘籍**
- ✅ **创建文章时的避坑指南**
- ✅ 一套标准化的发布流程
- ✅ 可随时查阅的维护手册

**从此写博客只需三步**：

```
hexo new "新文章"        # 写
hexo clean && hexo g -d  # 发
git push                 # 备（可选）
```

**域名永久保留**：

```
echo -n "你的域名" > source/CNAME  # 一次配置，永久有效
```

**创建文章避坑**：

```
# 稳妥的两步法
hexo new "文章标题"
notepad "source/_posts/文章标题.md"
```