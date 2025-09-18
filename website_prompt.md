# L1签证自评工具网站部署指南

## 目录
- [使用GitHub部署指南](#使用github部署指南)
  - [部署React版本](#部署react版本)
  - [部署HTML版本](#部署html版本)
- [功能说明](#功能说明)
- [常见问题解决](#常见问题解决)

## 使用GitHub部署指南

### 部署React版本

#### 准备工作
1. **安装必要软件**
   - 安装Node.js：访问 [Node.js官网](https://nodejs.org/) 下载并安装最新LTS版本
   - 安装Git：访问 [Git官网](https://git-scm.com/) 下载并安装

2. **创建GitHub账号和仓库**
   - 访问 [GitHub](https://github.com/) 注册账号并登录
   - 点击右上角"+"号，选择"New repository"创建新仓库
   - 填写仓库名称（如 "l1-visa-assessment"）
   - 选择"Public"仓库类型
   - 勾选"Initialize this repository with a README"
   - 点击"Create repository"按钮

#### 本地设置与构建
1. **克隆仓库到本地**
   ```bash
   git clone https://github.com/your-username/your-repository-name.git
   cd your-repository-name
   ```

2. **将项目文件复制到仓库目录**
   - 将整个项目文件复制到刚才克隆的仓库目录中

3. **安装依赖并构建项目**
   ```bash
   npm install
   npm run build
   ```
   这将在项目根目录生成一个 `dist` 文件夹，包含所有构建后的静态文件

#### 部署到GitHub Pages

**方法一：手动部署**
1. **创建gh-pages分支**
   ```bash
   git checkout -b gh-pages
   ```

2. **复制构建文件到根目录**
   ```bash
   cp -r dist/* .
   ```

3. **提交更改并推送到GitHub**
   ```bash
   git add .
   git commit -m "Deploy to GitHub Pages"
   git push origin gh-pages
   ```

4. **配置GitHub Pages**
   - 在GitHub仓库页面，点击"Settings" -> "Pages"
   - 在"Source"下拉菜单中选择"gh-pages"分支，然后选择"/ (root)"
   - 点击"Save"按钮
   - 几分钟后，您的网站将可以通过 `https://your-username.github.io/your-repository-name/` 访问

**方法二：使用GitHub Actions自动部署**
1. **在项目根目录创建GitHub Actions配置文件**
   - 创建 `.github/workflows/deploy.yml` 文件
   - 粘贴以下内容：
   ```yaml
   name: Deploy to GitHub Pages

   on:
     push:
       branches: [ main ]  # 监听main分支的推送

   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         - name: Use Node.js
           uses: actions/setup-node@v2
           with:
             node-version: '18'  # 使用Node.js 18版本
         - name: Install dependencies
           run: npm ci
         - name: Build
           run: npm run build
         - name: Deploy to GitHub Pages
           uses: peaceiris/actions-gh-pages@v3
           with:
             github_token: ${{ secrets.GITHUB_TOKEN }}
             publish_dir: ./dist
   ```

2. **提交配置文件并推送到GitHub**
   ```bash
   git add .github/workflows/deploy.yml
   git commit -m "Add GitHub Actions for deployment"
   git push origin main
   ```

3. **配置GitHub Pages**
   - 在GitHub仓库页面，点击"Settings" -> "Pages"
   - 在"Source"下拉菜单中选择"gh-pages"分支，然后选择"/ (root)"
   - 点击"Save"按钮

### 部署HTML版本

如果您只想部署简单的HTML版本（根目录下的.html文件），可以使用以下方法：

1. **将HTML文件复制到仓库**
   - 将所有.html文件（index_html.html, questionnaire.html, result.html等）复制到GitHub仓库目录

2. **提交并推送更改**
   ```bash
   git add .
   git commit -m "Add HTML version"
   git push origin main
   ```

3. **配置GitHub Pages**
   - 在GitHub仓库页面，点击"Settings" -> "Pages"
   - 在"Source"下拉菜单中选择"main"分支，然后选择"/ (root)"
   - 点击"Save"按钮

4. **重命名主页文件**
   - 将 `index_html.html` 重命名为 `index.html`，这样GitHub Pages会自动将其作为主页
   - 或者在GitHub Pages设置中指定 `index_html.html` 作为默认页面

## 功能说明

### React版本功能
- **完整的问卷系统**：个人背景、中国公司情况和美国公司情况三个部分
- **智能评分算法**：根据用户回答计算各维度得分
- **数据可视化**：使用雷达图展示各维度得分情况
- **优势项和风险项分析**：识别用户的优势和风险点
- **个性化优化建议**：提供短期、中期和长期改进建议
- **客户管理系统**：管理员可以查看提交的客户信息

### HTML版本功能
- 基本的问卷系统
- 简单的评分计算
- 结果展示

## 常见问题解决

### 1. 页面路由问题
如果使用React版本部署后出现路由问题（刷新页面后404），解决方案：
- 在GitHub Pages设置中，确保您使用的是gh-pages分支
- 对于React Router v7，需要确保路由配置正确
- 考虑使用HashRouter替代BrowserRouter以避免路由问题

### 2. 构建失败
- 确保Node.js版本正确（推荐v16或更高）
- 检查依赖是否完全安装 (`npm install`)
- 查看详细错误信息并针对性解决

### 3. 数据存储问题
- 网站使用localStorage存储数据，这是浏览器本地存储，不会上传到服务器
- 确保浏览器允许使用localStorage

### 4. 图表显示问题
- 如果雷达图无法显示，检查recharts库是否正确安装和导入
- 确认数据格式正确且不为空

通过以上步骤，您应该能够成功在GitHub平台部署L1签证自评工具网站。如果遇到任何问题，可以参考GitHub Pages文档或在GitHub社区寻求帮助。