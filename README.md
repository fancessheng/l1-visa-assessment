# 美福环球L1签证自评工具 - 部署指南

## 网站发布步骤

### 1. 准备工作
- 确保已安装Node.js和pnpm
- 拥有GitHub账号和仓库
- 注册Netlify或Vercel账号

### 2. 构建项目
```bash
# 安装依赖
pnpm install

# 构建生产版本
pnpm build
```
构建完成后会生成`dist`文件夹，包含所有静态文件

### 3. 部署选项

#### 选项A: 使用Netlify部署
1. 将代码推送到GitHub仓库
2. 在Netlify中导入项目仓库
3. 配置构建设置:
   - 构建命令: `pnpm build`
   - 发布目录: `dist/static`
4. 点击"部署网站"，等待完成

#### 选项B: 使用Vercel部署
1. 将代码推送到GitHub仓库
2. 在Vercel中导入项目
3. 配置项目设置:
   - 框架预设: `Vite`
   - 构建命令: `pnpm build`
   - 输出目录: `dist/static`
4. 点击"部署"，等待完成

### 4. 访问网站
部署完成后，Netlify/Vercel会提供一个随机域名，您也可以绑定自定义域名。

客户信息管理页面访问地址: `https://[您的域名]/kehu.html`
登录账号: admin
登录密码: admin

### 注意事项
- 本项目为纯前端应用，客户数据存储在浏览器localStorage中
- 如需数据持久化存储，需额外开发后端服务
- 部署前请确保所有功能正常运行