# Event Matrix - 赛事矩阵

一个赛博朋克风格的赛事日历应用，用于追踪各类竞赛活动（CTF、机器人竞赛、编程比赛等），并提供邮件提醒功能。

![赛博朋克风格](https://img.shields.io/badge/style-cyberpunk-neon?color=%2300e5ff)
![React](https://img.shields.io/badge/frontend-React_18-61dafb?logo=react)
![Node.js](https://img.shields.io/badge/backend-Node.js-339933?logo=node.js)
![MySQL](https://img.shields.io/badge/database-MySQL-4479a1?logo=mysql)
![Capacitor](https://img.shields.io/badge/mobile-Capacitor_Android-119eff?logo=capacitor)

## 功能特性

- **日历视图** - 赛博朋克风格月历，右键菜单快速查看当日赛事
- **列表视图** - 搜索/过滤赛事，快速定位感兴趣的活动
- **赛事管理** - CRUD操作，自定义滚轮式日期时间选择器
- **权重系统** - 记录赛事重要程度（低/中/高/顶级）
- **难度分类** - 入门/简单/中等/困难/专家五个等级
- **邮件提醒** - 月提醒/周提醒/日提醒，年度赛事预测提醒
- **年度赛事预测** - 去年举办过的赛事，今年可能再举办时提前提醒关注
- **用户系统** - 注册/登录/邮箱验证/角色权限管理
- **首次初始化** - 新部署时引导创建超级管理员账号
- **后台管理** - 用户管理、系统设置
- **移动端适配** - Android APK，横竖屏自适应，iOS安全区域
- **全局右键菜单** - 替换原生菜单，统一赛博朋克风格
- **反馈功能** - 快速跳转GitHub Issues提交反馈

## 用户角色系统

| 角色          | 权限说明                               |
| ------------- | -------------------------------------- |
| `super_admin` | 最高权限，管理所有用户、赛事、系统设置 |
| `admin`       | 管理普通用户和赛事，不能管理超管       |
| `user`        | 基础功能，可管理自己创建的赛事         |

### 赛事操作权限

- **超管创建的赛事**：只有创建者可修改/删除
- **管理员创建的赛事**：所有管理员和超管可修改/删除
- **用户创建的赛事**：管理员、超管和创建者可修改/删除
- **未知创建者的赛事**：管理员和超管可修改/删除

## 技术栈

| 层级     | 技术                         |
| -------- | ---------------------------- |
| 前端     | React 18 + 自定义赛博朋克CSS |
| 后端     | Node.js + Express            |
| 数据库   | MySQL                        |
| 邮件     | Nodemailer (SMTP)            |
| 定时任务 | node-cron                    |
| 移动端   | Capacitor (Android)          |

## 核心组件

### WheelPicker - 滚轮选择器

自定义滚轮式选择器，模拟手机闹钟时间选择体验：

- 无限循环滚动模式
- 精确高亮框对齐
- 拖拽和点击交互
- 非无限模式边界padding处理

### GlobalContextMenu - 全局右键菜单

替换原生浏览器右键菜单：

- 智能四边界检测，防止溢出屏幕
- 不同区域显示不同菜单项
- 日期区域：显示当日赛事列表 + 快捷操作

### DateTimePicker - 日期时间选择器

替换原生 `datetime-local`，解决浏览器兼容性问题：

- 年/月/日/时/分 5个滚轮选择器
- 分钟每5分钟一个选项
- 月份天数自动调整

## 快速开始

### 1. 环境要求

- Node.js 16+
- MySQL 5.7+ / 8.0+
- Java 17 (Android构建可选)

### 2. 安装依赖

```bash
# 克隆项目
git clone https://github.com/PeakSec/Event-Matrix.git
cd Event-Matrix

# 安装后端依赖
cd backend
npm install

# 安装前端依赖
cd ../frontend
npm install
```

### 3. 配置环境变量

复制 `backend/.env.example` 到 `backend/.env` 并填写配置：

```env
# 服务端口
PORT=3001

# 数据库配置
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=your_password
MYSQL_DATABASE=event_calendar

# 邮件服务
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password

# JWT密钥
JWT_SECRET=your_jwt_secret

# 前端地址
FRONTEND_URL=http://localhost:3003

# 环境
NODE_ENV=development
```

### 4. 启动服务

```bash
# 启动后端 (开发模式，端口 3001)
cd backend
npm run dev

# 启动前端 (端口 3003)
cd frontend
npm start
```

### 5. 访问应用

打开浏览器访问 http://localhost:3003

### 6. 首次初始化

**新部署时**：系统检测到没有用户，会显示初始化弹窗，引导你创建首个超级管理员账号。

**已有数据时**：直接显示登录界面。

## 测试账号 (仅开发环境)

| 账号  | 密码        | 角色  | 说明               |
| ----- | ----------- | ----- | ------------------ |
| admin | admin123456 | admin | 仅开发环境自动创建 |
| test  | test123456  | user  | 仅开发环境自动创建 |

**超级管理员**：需要通过首次初始化流程创建，输入自己的邮箱。

**注意**: 

- 生产环境不会自动创建任何账号
- 已存在的账号不会被更新或删除
- 超管账号请使用自己的真实邮箱

## API接口

### 初始化 (首次部署)

- `GET /api/init/status` - 检查系统是否需要初始化
- `POST /api/init/super-admin` - 创建首个超级管理员账号（仅无用户时可调用）

### 认证相关

- `POST /api/auth/register` - 用户注册
- `POST /api/auth/login` - 用户登录
- `GET /api/auth/verify-email` - 验证邮箱
- `POST /api/auth/resend-verification` - 重发验证邮件

### 赛事管理

- `GET /api/events` - 获取所有赛事
- `GET /api/events/upcoming` - 获取即将开始的赛事
- `GET /api/events/:id` - 获取单个赛事详情
- `POST /api/events` - 创建新赛事
- `PUT /api/events/:id` - 更新赛事信息
- `DELETE /api/events/:id` - 删除赛事

### 用户相关

- `GET /api/user/me` - 获取当前用户信息
- `PUT /api/user/profile` - 更新用户资料
- `POST /api/user/password/request-code` - 请求修改密码验证码
- `POST /api/user/password/change-with-code` - 使用验证码修改密码

### 管理后台 (需要管理员权限)

- `GET /api/admin/users` - 获取用户列表
- `PUT /api/admin/users/:id/role` - 更新用户角色
- `PUT /api/admin/users/:id/verify-email` - 手动验证用户邮箱
- `DELETE /api/admin/users/:id` - 删除用户
- `GET /api/admin/stats` - 获取统计数据

## Android APK构建

```bash
# 在 frontend 目录下执行完整流程

# 1. 构建前端
npm run build

# 2. 同步到Android
npx cap sync android

# 3. 构建APK (需要Java 17)
cd android
./gradlew assembleDebug

# 4. 签名 (Target SDK 36+必须使用v2签名方案)
zipalign -v 4 app-debug.apk aligned.apk
apksigner sign --ks my-debug.p12 --ks-pass pass:android --out final.apk aligned.apk
```

## UI主题风格

赛博朋克玻璃霓虹风格：

| 颜色   | 用途          | 色值      |
| ------ | ------------- | --------- |
| 霓虹青 | 主色调        | `#00e5ff` |
| 霓虹金 | 高亮/今天标签 | `#ffd700` |
| 霓虹粉 | 强调/困难级别 | `#ff4081` |
| 霓虹绿 | 成功/入门级别 | `#00e676` |

## 项目结构

```
Event-Matrix/
├── backend/                 # 后端服务
│   ├── controllers/         # API控制器
│   ├── db/                  # 数据库配置
│   ├── middleware/          # 中间件
│   ├── routes/              # 路由定义
│   └── server.js            # 入口文件
│
├── frontend/                # 前端应用
│   ├── src/
│   │   ├── components/      # React组件
│   │   │   ├── Calendar.js          # 日历组件
│   │   │   ├── WheelPicker.js       # 滚轮选择器
│   │   │   ├── DateTimePicker.js    # 日期时间选择器
│   │   │   ├── EventModal.js        # 赛事编辑弹窗
│   │   │   ├── GlobalContextMenu.js # 全局右键菜单
│   │   │   └── ...
│   │   ├── styles/          # CSS样式
│   │   ├── utils/           # 工具函数
│   │   └── App.js           # 主应用
│   ├── public/              # 静态资源
│   └── capacitor.config.ts  # Capacitor配置
│
└── README.md
```

## 配置说明

| 配置项           | 默认值 | 说明                       |
| ---------------- | ------ | -------------------------- |
| 图形验证码有效期 | 5分钟  | 登录/注册验证码            |
| 邮箱验证码有效期 | 2分钟  | 邮箱验证/密码重置          |
| 验证码发送间隔   | 120秒  | 防止频繁发送               |
| 提醒检查时间     | 12:00  | 每天定时检查提醒（可配置） |

### 提醒类型

| 类型         | 说明                                                 |
| ------------ | ---------------------------------------------------- |
| 月提醒       | 今年赛事提前一个月提醒备赛                           |
| 周提醒       | 赛事开始前一周提醒备赛                               |
| 日提醒       | 赛事开始前一天提醒准备                               |
| 年度赛事预测 | 去年举办过但今年还没添加的赛事，提醒用户关注官方信息 |

## 开发计划

- [ ] 支持更多赛事平台集成（CTFtime API、机器人竞赛官网等）
- [ ] 添加团队协作功能
- [ ] 支持iCal/CSV导出
- [ ] 多语言支持
- [ ] iOS版本

## 许可证

MIT License

## 作者

J1Zh0n9

## 致谢

感谢所有为各类竞赛活动做出贡献的组织和个人。
