---
title: 知识库完整使用与发布流程
description: 从 Obsidian 新建笔记，到 Quartz 本地预览、Git 提交、GitHub Pages 发布与故障排查
tags:
  - 知识库
  - Obsidian
  - Quartz
  - GitHub-Pages
---

# 知识库完整使用与发布流程

这份说明针对当前这个项目。第一次操作时建议从头读一遍；以后可以直接看[日常更新速查](#一日常更新速查)。

## 一、日常更新速查

每次只新增或修改笔记时，照以下顺序操作：

1. 用 Obsidian 打开 `D:\code\study\3D-reconstruction-notes\content`，新建或修改笔记并保存。
2. 如果想先检查网页效果，在项目根目录运行 `npm run preview`，打开 `http://localhost:8080/`。已有预览服务时直接刷新网页。
3. 在 Windows **CMD** 中进入项目根目录，检查改动：

   ```bat
   cd /d D:\code\study\3D-reconstruction-notes
   git status
   git diff -- content
   ```

4. 确认改动可以公开后，提交并推送：

   ```bat
   git add -A
   git status
   git commit -m "docs: 更新学习笔记"
   git push origin v5
   ```

5. 打开 [GitHub Actions](https://github.com/HelloWorld77777777/3D-reconstruction-notes/actions)，等待最新一次 `Deploy Quartz to GitHub Pages` 显示绿色成功，再打开[公开网站](https://helloworld77777777.github.io/3D-reconstruction-notes/)检查页面。

`git add -A` 会加入本仓库中所有未忽略的改动。执行前务必检查 `git status`；不要把密码、访问令牌、私人资料或不准备公开的草稿推送出去。如果只改了笔记，也可以用 `git add content` 只暂存笔记目录。

## 二、项目和网址分别是什么

| 项目            | 当前地址或位置                                                                                                         | 用途                                                      |
| --------------- | ---------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| 项目根目录      | `D:\code\study\3D-reconstruction-notes`                                                                                | Git 仓库、Quartz 配置、部署文件所在位置；在这里运行命令。 |
| Obsidian 知识库 | `D:\code\study\3D-reconstruction-notes\content`                                                                        | Markdown 笔记所在位置；Obsidian 应打开这一层。            |
| 网站首页源文件  | `content/index.md`                                                                                                     | 修改网站首页介绍和栏目入口。                              |
| 本地预览        | `http://localhost:8080/`                                                                                               | 只供当前电脑查看尚未发布的内容。                          |
| 公开网站        | [helloworld77777777.github.io/3D-reconstruction-notes/](https://helloworld77777777.github.io/3D-reconstruction-notes/) | 发布成功后任何人都可以访问。                              |
| GitHub 仓库     | [HelloWorld77777777/3D-reconstruction-notes](https://github.com/HelloWorld77777777/3D-reconstruction-notes)            | 保存源码、笔记和修改历史。                                |
| 自动部署        | [仓库 Actions 页面](https://github.com/HelloWorld77777777/3D-reconstruction-notes/actions)                             | 查看每次构建和发布是否成功。                              |

**注意路径区别：** 本地预览的网址从 `http://localhost:8080/` 开始，后面**不要**加 `/3D-reconstruction-notes/`；公开网站则必须带仓库路径 `/3D-reconstruction-notes/`。把公开网址中的路径直接复制到本地 `localhost` 后，可能看到 404。

## 三、文件夹和文件的作用

当前目录大致如下。每个分类的 `index.md` 是该分类的首页；`content/index.md` 是整个网站的首页。

```text
3D-reconstruction-notes/
├─ README.md                         GitHub 仓库首页说明
├─ package.json                      本地预览等 npm 命令
├─ quartz.config.yaml                网站标题、网址、插件等配置
├─ .github/
│  └─ workflows/
│     └─ deploy.yml                  GitHub Pages 自动部署流程
└─ content/                           Obsidian 知识库，也是网站内容源
   ├─ index.md                        网站首页
   ├─ 01-C++/
   │  ├─ index.md                     C++ 分类首页
   │  └─ 01-C++基础入门.md             示例学习笔记
   ├─ 02-数学与多视图几何/index.md
   ├─ 03-CUDA/index.md
   ├─ 04-3DGS源码解析/index.md
   ├─ 05-工程项目/index.md
   ├─ 06-Bug与工程经验/index.md
   ├─ 07-每周学习复盘/index.md
   └─ 08-知识库使用与发布/
      ├─ index.md                     本栏首页
      └─ README.md                    正在阅读的完整流程
```

`node_modules/` 是本机安装的依赖，`public/` 是构建产物，`.obsidian/` 是本机 Obsidian 设置；它们已被 Git 忽略，不需要手动上传。网站实际发布的内容来自 GitHub Actions 构建出的 `public/`，不是直接把 `content/` 当成静态网页上传。

## 四、用 Obsidian 写和整理笔记

### 4.1 打开正确的知识库

1. 打开 Obsidian。如果尚未打开本知识库，选择“打开本地文件夹作为知识库”。
2. 选中 `D:\code\study\3D-reconstruction-notes\content`。选的是 **content**，不要选整个仓库，也不要选 `public`。
3. 左侧文件列表应看到 `index.md` 和 `01-C++` 至 `08-知识库使用与发布` 的文件夹。

Obsidian 保存的 `.md` 文件就是 Quartz 使用的文件。保存之后，本机文件已经更新；这时还没有发布到公网。

### 4.2 在现有分类中新建笔记

以 C++ 分类为例：

1. 在 Obsidian 左侧右键单击 `01-C++`，选择“新建笔记”。
2. 输入一个清晰且不重复的文件名，例如 `02-指针与引用`。实际文件位于 `content/01-C++/02-指针与引用.md`。
3. 写入以下模板，再按实际内容修改：

   ````markdown
   ---
   title: 指针与引用
   date: 2026-10-23
   tags:
     - cpp
     - programming
   ---

   # 指针与引用

   ## 学习目标

   用一两句话说明这篇笔记要解决的问题。

   ## 核心概念

   记录定义、图示、代码和自己的理解。

   ## 示例

   ```cpp
   int value = 42;
   int* pointer = &value;
   int& reference = value;
   ```

   ## 常见错误

   记录容易混淆的点和修正方法。

   ## 参考资料

   - 在这里添加资料链接和自己的备注。
   ````

4. 保存后，可以在分类首页 `content/01-C++/index.md` 增加 `[[02-指针与引用|指针与引用]]`，方便读者找到它。
5. 用本地预览点开这篇笔记，确认标题、代码块和链接正常。

`title` 是页面标题；文件名决定链接路径；`date` 和 `tags` 可以按需要填写。Windows 文件名不能包含 `\ / : * ? " < > |` 等字符。代码块的语言标记（如 `cpp`、`python`）写在开头三个反引号后，网站才能高亮。

### 4.3 修改已有笔记和链接

打开笔记编辑并保存即可。要从一篇笔记跳到另一篇，可使用 Obsidian 的 Wiki 链接：

```markdown
[[01-C++/01-C++基础入门|C++ 基础入门]]
```

`|` 左侧是目标文件在 `content` 中的路径（省略 `.md`），右侧是读者看到的文字。给分类首页建链接时使用 `[[03-CUDA/index|CUDA 高性能计算]]`。如果重命名或移动笔记，检查指向它的链接并在本地网页中重新点开验证。

### 4.4 新增分类

1. 在 `content` 下新建分类文件夹，例如 `09-论文阅读`。
2. 在该文件夹中创建 `index.md`，写上 YAML 中的 `title`，再写分类简介和笔记导航。
3. 在 `content/index.md` 增加指向 `[[09-论文阅读/index|09 · 论文阅读]]` 的入口。
4. 在本地预览中点击首页入口，检查分类页和分类内笔记都能打开。
5. 提交并推送后，再检查公开网站。

空文件夹不会单独被 Git 保存；有 `index.md` 后，分类才有可提交的内容和可访问的首页。

## 五、在本机预览网站

### 5.1 启动预览

在 **CMD** 中运行：

```bat
cd /d D:\code\study\3D-reconstruction-notes
npm run preview
```

第一次运行前如果依赖尚未安装，先在项目根目录执行 `npm ci`。这个步骤依赖 `package-lock.json`，通常只需在新电脑首次使用或依赖变化后执行。本项目需要 Node.js 22 或更高版本；自动部署使用独立的 Node.js 24 环境。

命令运行后，打开 `http://localhost:8080/`。预览服务通常会监视笔记改动并重新构建；保存笔记后，稍等片刻并刷新页面。若 8080 端口已经有本项目的预览服务运行，直接使用现有服务即可，不要重复启动。要停止当前 CMD 窗口中的服务，按 `Ctrl + C`。

`npm run preview` 将本地预览文件写入 `.quartz/preview/`；`npx quartz build` 将正式构建文件写入 `public/`。两者使用不同目录，避免预览时运行正式构建导致 Explorer 链接误带 GitHub Pages 路径。如果修改了 `package.json` 里的预览命令，先停止旧的预览服务再重新运行 `npm run preview`。

### 5.2 预览时重点检查什么

- 从首页依次打开需要检查的分类，再点开新增笔记。
- 点击新加的内部链接，确认目标不是 404。
- 检查标题、列表、代码块、图片及公式显示是否正常。
- 图片尽量放在 `content` 内，并用 Obsidian 链接或相对路径引用；上传前确认图片文件也出现在 `git status` 中。
- 在手机或较窄浏览器窗口里浏览一次重要页面，检查是否容易阅读。

本地预览成功只说明本机能构建。要让其他人访问，还要把改动推送到 GitHub 并等待 Actions 成功。

## 六、把更新发布到 GitHub Pages

### 6.1 检查当前分支和改动

在 **CMD** 中运行：

```bat
cd /d D:\code\study\3D-reconstruction-notes
git branch --show-current
git status
git diff
```

当前部署流程监听 `v5` 分支；`git branch --show-current` 应显示 `v5`。`git status` 列出新增、修改或删除的文件。`git diff` 显示已跟踪文件尚未暂存的内容；新文件需要结合 `git status` 和编辑器查看。确认这些内容都适合公开。不要把研究资料里的账号密码、访问令牌、私有数据或尚未获准发布的内容放进公开仓库。

如果 `git status` 显示 `nothing to commit, working tree clean`，本次没有未提交的本地改动；先确认 Obsidian 编辑的是这个项目的 `content`，而不是另一份同名知识库。

### 6.2 暂存、提交和推送

确认无误后运行：

```bat
git add -A
git status
git diff --cached --stat
git commit -m "docs: 添加本周学习记录"
git push origin v5
```

这几步分别表示：把改动选进本次提交、再次确认将要提交的文件、查看提交范围、保存一条 Git 历史记录、把 `v5` 分支推送到 GitHub。提交说明尽量描述实际改动，例如 `docs: 补充相机模型笔记`。如果只想提交某一篇笔记，可以用 `git add "content/02-数学与多视图几何/相机模型.md"` 指定文件；文件名不同要相应替换。

`git commit` 只保存在本机；**`git push origin v5` 成功后**，GitHub 才收到新提交。不要使用 `git push --force` 覆盖远端历史。

### 6.3 检查自动部署结果

这个项目的 `.github/workflows/deploy.yml` 会在 `v5` 分支收到推送时自动运行。它依次检出代码、设置 Node.js 24、执行 `npm ci`、安装 Quartz 插件、构建网站、上传构建结果并发布 GitHub Pages。

1. 打开 [Actions 页面](https://github.com/HelloWorld77777777/3D-reconstruction-notes/actions)。
2. 找到刚刚推送对应的 `Deploy Quartz to GitHub Pages` 运行记录。
3. 等待 `build` 和 `deploy` 都显示绿色成功。黄色表示还在运行；红色表示失败，可点进去查看失败步骤和日志。
4. 打开 [公开网站](https://helloworld77777777.github.io/3D-reconstruction-notes/)，从首页进入刚更新的页面。若浏览器显示旧内容，稍等片刻后刷新。

仓库的 Pages 发布源应设为 **GitHub Actions**。日常更新时一般不需要重新设置 Pages，也不需要手动上传 `public` 文件夹。

### 6.4 如何把网址发给别人

网站首页：`https://helloworld77777777.github.io/3D-reconstruction-notes/`。这是公网地址，可以复制给别人。分类和笔记的网址可在公开网站中点开页面后，从浏览器地址栏复制。`http://localhost:8080/` 只指向你自己的电脑，别人无法用它访问你的预览服务。

## 七、常见问题与处理顺序

| 现象                  | 先检查什么                                                           | 处理方法                                                                 |
| --------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| 本地链接打开 404      | 地址是否写成 `localhost:8080/3D-reconstruction-notes/...`            | 本地从 `http://localhost:8080/` 进入并点链接；本地网址不带仓库前缀。     |
| 公开网站的笔记 404    | Actions 是否成功；文件是否在 `content`；内部链接是否写对路径与文件名 | 确认文件已提交并推送，检查大小写与特殊字符，再从网站首页逐级点入。       |
| 修改了笔记但网站没变  | `git status`、`git log -1 --oneline`、最新 Actions 运行状态          | 可能尚未提交或推送、部署仍在运行，或浏览器缓存了旧页面。                 |
| `git status` 没有改动 | Obsidian 打开的知识库路径                                            | 确认写的是 `D:\code\study\3D-reconstruction-notes\content`。             |
| `git push` 被拒绝     | 是否在 `v5`，远端是否有另一台电脑的新提交                            | 先查看 `git status` 和远端变化；合并或变基前确认当前改动。不要强制推送。 |
| Actions 构建失败      | 失败的是 `npm ci`、插件安装还是 `Build Quartz`                       | 打开失败步骤看具体报错，针对报错修复笔记、依赖或配置，再推送新提交。     |
| 本地端口 8080 被占用  | 是否已有一个预览服务                                                 | 优先复用已有预览窗口；不用重复运行。                                     |
| 新分类没出现在首页    | 是否创建该分类的 `index.md` 并修改 `content/index.md`                | 给分类写首页和首页入口，构建后点击验证。                                 |

遇到 404 时，按这个顺序查：**从正确的首页进入 → 确认文件存在 → 确认链接路径 → 确认 GitHub Actions 成功 → 重新打开公开网址**。文件实际路径与网页路径不一定完全相同，最好从网站内点击得到可用网址，不要手工猜带中文或 `C++` 的网址。

## 八、维护与安全习惯

- 每次推送前看 `git status`，尤其留意误加入的图片、数据集、临时文件和私人资料。公开仓库中的已推送内容可能被别人下载；删除后也可能留在历史记录里。
- 修改网站首页时编辑 `content/index.md`；修改分类首页时编辑相应文件夹的 `index.md`；不要误改 `public/` 中的生成页面。
- 对重要改动使用具体的提交说明。需要了解最近更新时运行 `git log --oneline -5`；需要看尚未提交的文字改动时运行 `git diff`。
- 从另一台电脑继续写作前，先确认那台电脑的仓库与 `v5` 分支已同步。遇到冲突时先保留和检查双方内容，不要直接覆盖。
- 大型图片和附件会增加仓库体积；上传前确认确实需要公开且有使用权。
- 如果将来修改仓库名、GitHub 用户名或部署路径，需要同时检查 `quartz.config.yaml` 中的 `baseUrl`、Pages 设置，以及本文里的网址。

## 九、在新电脑上继续维护

本节用于电脑重装或换电脑；当前电脑已经有项目，无需重复克隆。

1. 安装 Git、Node.js（22 或更高版本）和 Obsidian。先在 CMD 中运行 `git --version`、`node --version`、`npm --version`，确认命令可用。
2. 在想存放项目的目录克隆 `v5` 分支。下面仍使用当前项目所在位置作为例子：

   ```bat
   cd /d D:\code\study
   git clone --branch v5 https://github.com/HelloWorld77777777/3D-reconstruction-notes.git
   cd /d D:\code\study\3D-reconstruction-notes
   npm ci
   ```

3. 在 Obsidian 中打开克隆后的 `content` 文件夹；用 `npm run preview` 检查首页和一篇笔记。
4. 写笔记前先运行 `git status`，确认分支为 `v5` 且工作区状态清楚。如果这期间另一台电脑又推送了新内容，应先同步并检查远端更新，再继续编辑。

如果新电脑使用了不同的磁盘或文件夹，需要把本文命令里的 `D:\code\study\3D-reconstruction-notes` 替换成新电脑上的真实路径。GitHub Pages 的公网地址不因本地文件夹位置变化而变化。

## 十、首次部署设置与流程文件

当前仓库已经配置并发布，本节用于以后核对设置，不需要每次更新都操作。

1. 在 GitHub 打开仓库的 **Settings → Pages**。
2. 在 **Build and deployment** 下将 **Source** 设为 **GitHub Actions**。
3. 确认仓库中存在 `.github/workflows/deploy.yml`，且其 `push` 触发分支为 `v5`。
4. 确认 `quartz.config.yaml` 中的 `baseUrl` 为 `HelloWorld77777777.github.io/3D-reconstruction-notes`。它和公开网址对应，不能写成本地 `localhost` 地址。
5. 推送到 `v5` 后，查看 Actions 中的构建和部署记录。工作流也支持在 Actions 页面手动选择 **Run workflow**，但日常更新直接推送即可。

流程文件中的关键阶段如下：

| 阶段 | 当前配置                                   | 作用                                                          |
| ---- | ------------------------------------------ | ------------------------------------------------------------- |
| 触发 | 推送到 `v5` 或手动运行                     | 决定何时重建网站。                                            |
| 环境 | `ubuntu-latest`、Node.js 24                | 在 GitHub 的独立机器上构建；本机 Node.js 版本不会直接控制它。 |
| 依赖 | `npm ci`、`npx quartz plugin install`      | 按锁定文件安装项目依赖和 Quartz 插件。                        |
| 构建 | `npx quartz build`                         | 把 `content` 中的 Markdown 生成为 `public` 静态文件。         |
| 发布 | 上传 `public`，运行 `actions/deploy-pages` | 将生成的网站交给 GitHub Pages。                               |

如果将来改动工作流、Quartz 配置或依赖，建议先在本地构建并检查，再提交这些配置文件；不要只看笔记是否显示。

## 十一、命令速查

以下命令都应在项目根目录执行。`cd /d` 是 **CMD** 切换盘符和目录的写法；如果用 PowerShell，可直接运行 `Set-Location 'D:\code\study\3D-reconstruction-notes'`。

| 命令                   | 用途                                                   |
| ---------------------- | ------------------------------------------------------ |
| `git status`           | 查看分支、改动、暂存和待提交文件。                     |
| `git diff`             | 查看已跟踪文件尚未暂存的文字改动。                     |
| `git diff --cached`    | 查看已经暂存、即将提交的文字改动。                     |
| `git log --oneline -5` | 查看最近五条提交。                                     |
| `npm ci`               | 按锁定文件安装依赖。                                   |
| `npm run preview`      | 构建到 `.quartz/preview`，并在本机 8080 端口预览网站。 |
| `npx quartz build`     | 单次生成网站到 `public`，用于正式构建检查。            |
| `git add -A`           | 暂存仓库内所有未忽略的新增、修改和删除。               |
| `git commit -m "说明"` | 在本机保存一次提交。                                   |
| `git push origin v5`   | 将 `v5` 提交上传到 GitHub 并触发自动部署。             |

## 十二、相关官方资料

- [Quartz 官方文档](https://quartz.jzhao.xyz/)
- [Quartz 托管与 GitHub Pages 部署说明](https://quartz.jzhao.xyz/hosting)
- [GitHub Pages 发布源设置](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [GitHub Actions 入门](https://docs.github.com/en/actions/get-started/understand-github-actions)

本文描述的是当前仓库的实际设置。以后如果目录、分支或网址发生变化，应同步更新这份流程。
