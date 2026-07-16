# 教学计划在线查看

此目录为 GitHub Pages 部署目录。推送到 GitHub 后，将通过 Pages 生成公开访问链接。

## 文件结构

```
web/
├── index.html        # 网页主页面
├── app.js            # 核心逻辑（渲染表格、保存编辑、轮询更新）
├── style.css         # 页面样式
├── config.js         # 配置文件（GitHub 信息、Token、Worker URL）
├── data.json         # Excel 导出的数据（由 sync_excel.bat 自动生成推送）
└── editable_data.json # 网页端编辑数据（教师在线编辑后自动生成）
```

## 部署步骤

### 1. 创建 GitHub 仓库

1. 登录 [github.com](https://github.com)
2. 点击右上角 `+` → `New repository`
3. 仓库名：`excel-sync`（或任意名称）
4. 可见性：**Public**（GitHub Pages 免费版需要公开仓库）
5. 勾选 `Add a README file`
6. 点击 `Create repository`

### 2. 推送网页文件

在项目目录运行：

```bash
cd excel-web-sync
git init
git remote add origin https://github.com/你的用户名/excel-sync.git
git add web/
git commit -m "初始化网页文件"
git push -u origin main
```

### 3. 开启 GitHub Pages

1. 进入仓库 → `Settings` → `Pages`
2. Source 选择 `Deploy from a branch`
3. Branch 选择 `main`，文件夹选择 `/web`
4. 点击 `Save`
5. 等待 1-2 分钟，页面顶部会显示公开链接：
   `https://你的用户名.github.io/excel-sync/`

### 4. 修改 config.js

将 `config.js` 中的配置改为你的实际信息：

```javascript
const CONFIG = {
    githubUser: '你的GitHub用户名',
    githubRepo: 'excel-sync',
    githubBranch: 'main',
    githubToken: '你的GitHub Token',
    workerUrl: '',  // 不使用 Worker 则留空
    pollInterval: 10000,
    dataSource: '',  // 留空则自动拼接 GitHub Pages URL
};
```

### 5. 创建 GitHub Token

1. GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. 点击 `Generate new token`
3. Repository access：选择 `excel-sync` 仓库
4. Permissions：`Contents` → `Read and Write`
5. 生成后复制 Token，粘贴到 config.js

### 6. 推送配置

```bash
git add web/config.js
git commit -m "更新配置"
git push
```

### 7. 验证

浏览器打开 `https://你的用户名.github.io/excel-sync/`，应能看到教学计划表格。

## 数据同步说明

- **Excel → 网页**：双击 `sync_excel.bat`，自动导出 data.json 并推送到 GitHub
- **网页 → 网页**：教师在网页编辑后点击保存，写入 editable_data.json
- **网页 → Excel**：在 WPS/Excel 中运行 `PullEditableFromJson` 宏，拉取网页编辑数据

**网页只是在线共享视图，最终数据以本地 Excel 为准。**
