# Art Design Pro 二次开发指南

> 适用版本：v0.0.0 | 最后更新：2026-05-28

---

## 一、项目概览

### 1.1 项目简介

Art Design Pro 是一款基于 **Vue 3 + TypeScript + Element Plus** 的企业级后台管理系统模板。项目专注于用户体验和快速开发，基于 Element Plus 设计规范进行了视觉优化，提供亮色/暗色双主题、多布局模式、国际化支持等开箱即用的功能。

- **官方文档**：[https://www.lingchen.kim/art-design-pro/docs/](https://www.lingchen.kim/art-design-pro/docs/)
- **演示站点**：见官方文档

### 1.2 技术栈

| 层级 | 技术 | 版本 | 说明 |
| --- | --- | --- | --- |
| 框架 | Vue | ^3.5.12 | Composition API (`<script setup lang="ts">`) |
| 构建工具 | Vite | ^6.1.0 | ESM 原生模块开发 |
| 语言 | TypeScript | ~5.6.3 | 全量 TS |
| UI 组件库 | Element Plus | ^2.10.2 | 全局注册所有图标 |
| 状态管理 | Pinia | ^3.0.2 | + `pinia-plugin-persistedstate` 持久化 |
| 路由 | Vue Router | ^4.4.2 | Hash 模式 (`createWebHashHistory`) |
| HTTP 客户端 | Axios | ^1.7.5 | 封装拦截器/重试/错误处理 |
| 图表 | ECharts | ^5.6.0 | 仪表盘分析页图表 |
| 国际化 | vue-i18n | ^9.14.0 | 中/英双语 |
| CSS 预处理 | SCSS (Sass) | ^1.81.0 | 亮色/暗色双主题 |
| 代码规范 | ESLint + Prettier + Stylelint + Husky + Commitlint | - | Git 提交自动校验与格式化 |

### 1.3 核心功能

- 亮色 / 暗色 / 跟随系统三种主题
- 四种菜单布局：左侧、顶部、混合、双栏
- 三种菜单主题：Design、Dark、Light
- 多标签页（Worktab）
- 全局面包屑导航
- 路由级别权限鉴权（前端/后端两种权限模式）
- 按钮级别权限指令（`v-auth`、`v-roles`）
- 全局搜索（菜单搜索）
- 锁屏功能
- 快速入口
- 全屏 / 水印 / 色弱模式
- 中英文国际化
- ECharts 图表
- 富文本编辑器（WangEditor）
- Excel 导入导出
- 图片裁剪 / 二维码生成 / 拖拽排序

---

## 二、环境要求与启动

### 2.1 环境要求

| 工具    | 最低版本 | 说明                    |
| ------- | -------- | ----------------------- |
| Node.js | >= 18.x  | 推荐使用 20.x LTS       |
| pnpm    | >= 8.x   | 项目使用 pnpm workspace |

### 2.2 快速启动

```bash
# 1. 克隆项目（如果尚未克隆）
git clone <仓库地址>

# 2. 进入项目目录
cd art-design-pro

# 3. 安装依赖（必须使用 pnpm）
pnpm install

# 4. 启动开发服务器
pnpm dev

# 开发服务器默认运行在 http://localhost:5173
```

### 2.3 全部可用命令

| 命令                  | 说明                             |
| --------------------- | -------------------------------- |
| `pnpm dev`            | 启动开发服务器（自动打开浏览器） |
| `pnpm build`          | TypeScript 类型检查 + 生产构建   |
| `pnpm serve`          | 预览生产构建结果                 |
| `pnpm lint`           | ESLint 代码检查                  |
| `pnpm fix`            | ESLint 自动修复                  |
| `pnpm lint:prettier`  | Prettier 格式化所有文件          |
| `pnpm lint:stylelint` | Stylelint 样式检查并修复         |
| `pnpm commit`         | 交互式提交（cz-git）             |
| `pnpm clean:dev`      | 清理开发环境缓存                 |

### 2.4 环境变量说明

**开发环境** [`.env.development`](.env.development)：

```env
# 网站基础路径
VITE_BASE_URL = /

# API 接口地址（开发时使用 Mock 服务）
VITE_API_URL = https://m1.apifoxmock.com/m1/6400575-6097373-default

# 是否移除 console
VITE_DROP_CONSOLE = false

# Vite 端口号
VITE_PORT = 5173
```

**生产环境** [`.env.production`](.env.production)：

```env
# 网站基础路径（部署子路径时修改此项）
VITE_BASE_URL = /art-design-pro/

# API 接口地址（替换为你的后端地址）
VITE_API_URL = https://your-api-server.com

# 是否移除 console
VITE_DROP_CONSOLE = true

# Vite 端口号
VITE_PORT = 5173
```

### 2.5 路径别名

项目在 [`vite.config.ts`](vite.config.ts) 中配置了以下路径别名：

| 别名      | 映射路径             | 说明       |
| --------- | -------------------- | ---------- |
| `@`       | `src/`               | 源码根目录 |
| `@views`  | `src/views/`         | 页面组件   |
| `@imgs`   | `src/assets/img/`    | 图片资源   |
| `@icons`  | `src/assets/icons/`  | 图标资源   |
| `@utils`  | `src/utils/`         | 工具函数   |
| `@stores` | `src/store/`         | 状态管理   |
| `@styles` | `src/assets/styles/` | 样式文件   |

---

## 三、项目目录结构

```
art-design-pro/
├── .env                      # 通用环境变量
├── .env.development          # 开发环境变量
├── .env.production           # 生产环境变量
├── .husky/                   # Git Hooks 配置
├── .vscode/                  # VSCode 编辑器配置
├── documents/                # 项目文档
├── public/                   # 静态资源（不经过编译）
├── scripts/                  # 开发脚本
├── src/
│   ├── api/                  # API 接口层
│   │   ├── menuApi.ts        # 菜单接口
│   │   └── usersApi.ts       # 用户接口
│   ├── assets/
│   │   ├── icons/            # 图标资源（iconfont）
│   │   ├── styles/           # 全局样式
│   │   │   ├── app.scss      # 全局样式
│   │   │   ├── dark.scss     # 暗黑主题
│   │   │   ├── el-dark.scss  # Element Plus 暗黑主题
│   │   │   ├── el-light.scss # Element Plus 亮色主题
│   │   │   ├── el-ui.scss    # Element Plus 样式优化
│   │   │   └── change.scss   # 主题切换过渡效果
│   │   └── svg/              # SVG 资源
│   ├── components/           # 自定义组件
│   │   └── custom/           # 业务组件
│   ├── composables/          # 组合式函数（Composables）
│   │   ├── useAuth.ts        # 登录/注册/退出逻辑
│   │   ├── useCommon.ts      # 通用函数
│   │   ├── useTable.ts       # 表格 CRUD 逻辑
│   │   ├── useTableColumns.ts# 表格列配置
│   │   ├── useTheme.ts       # 主题切换逻辑
│   │   ├── useHeaderBar.ts   # 顶栏功能
│   │   ├── useChart.ts       # 图表通用逻辑
│   │   └── useFastEnter.ts   # 快速入口逻辑
│   ├── config/               # 系统配置
│   │   ├── index.ts          # 主配置文件（系统名称/主题/布局/颜色）
│   │   ├── fastEnter.ts      # 快速入口配置
│   │   ├── festival.ts       # 节日配置
│   │   └── headerBar.ts      # 顶栏功能配置
│   ├── directives/           # 自定义指令
│   │   ├── auth.ts           # v-auth 权限指令
│   │   ├── roles.ts          # v-roles 角色指令
│   │   ├── ripple.ts         # 涟漪效果指令
│   │   └── highlight.ts      # 代码高亮指令
│   ├── enums/                # 枚举定义
│   │   ├── appEnum.ts        # 系统级别枚举（主题/布局/语言）
│   │   └── formEnum.ts       # 表单相关枚举
│   ├── locales/              # 国际化
│   │   └── langs/
│   │       ├── zh.json       # 中文语言包
│   │       └── en.json       # 英文语言包
│   ├── mock/                 # Mock 数据
│   ├── router/               # 路由配置
│   │   ├── index.ts          # 路由实例创建
│   │   ├── routesAlias.ts    # 路由路径别名枚举
│   │   ├── guards/           # 路由守卫
│   │   │   ├── beforeEach.ts # 前置守卫（登录/权限/动态路由）
│   │   │   └── afterEach.ts  # 后置守卫
│   │   ├── routes/           # 路由定义
│   │   │   ├── staticRoutes.ts   # 静态路由（登录/404等）
│   │   │   └── asyncRoutes.ts    # 异步路由（业务菜单路由）
│   │   └── utils/            # 路由工具函数
│   ├── store/                # Pinia 状态管理
│   │   ├── index.ts          # Store 初始化
│   │   └── modules/
│   │       ├── user.ts       # 用户状态（登录/Token/语言）
│   │       ├── setting.ts    # 系统设置（主题/布局/菜单）
│   │       ├── menu.ts       # 菜单状态
│   │       ├── table.ts      # 表格状态
│   │       └── worktab.ts    # 标签页状态
│   ├── utils/                # 工具函数
│   │   ├── http/             # Axios 封装
│   │   │   ├── index.ts      # 请求实例/拦截器/重试
│   │   │   ├── status.ts     # HTTP 状态码
│   │   │   └── error.ts      # 错误处理
│   │   ├── storage/          # 存储工具
│   │   ├── navigation/       # 导航工具（面包屑/标签页）
│   │   ├── theme/            # 主题工具
│   │   ├── table/            # 表格工具
│   │   ├── sys/              # 系统工具（事件总线/升级）
│   │   ├── dataprocess/      # 数据处理（数组/格式化）
│   │   ├── ui/               # UI 工具（颜色/Emoji/Loading）
│   │   ├── validation/       # 表单验证
│   │   └── constants/        # 常量定义
│   ├── views/                # 页面组件
│   │   ├── index/            # 主布局容器
│   │   ├── auth/             # 认证页面（登录/注册/忘记密码）
│   │   ├── dashboard/        # 仪表盘（控制台/分析/电商）
│   │   ├── system/           # 系统管理（用户/角色/菜单）
│   │   ├── article/          # 文章管理
│   │   ├── template/         # 模板页面
│   │   ├── widgets/          # 小组件页面
│   │   ├── examples/         # 示例页面
│   │   ├── exception/        # 异常页面（403/404/500）
│   │   ├── result/           # 结果页面（成功/失败）
│   │   ├── change/           # 更新日志
│   │   ├── safeguard/        # 服务器监控
│   │   └── outside/          # 外部页面（iframe）
│   ├── App.vue               # 根组件
│   ├── main.ts               # 应用入口
│   └── env.d.ts              # 类型声明
├── index.html                # HTML 入口
├── package.json              # 项目依赖配置
├── vite.config.ts            # Vite 构建配置
├── tsconfig.json             # TypeScript 配置
├── eslint.config.mjs         # ESLint 配置
├── .prettierrc               # Prettier 配置
├── .stylelintrc.cjs          # Stylelint 配置
└── commitlint.config.cjs     # Commitlint 配置
```

---

## 四、核心文件详解（二次开发必看）

### 4.1 入口与应用配置

#### [`src/main.ts`](src/main.ts) — 应用入口

应用启动文件，负责初始化 Vue 实例、注册全局插件。关键内容：

- 注册 Pinia Store、Vue Router
- 注册全局自定义指令（`v-auth`、`v-roles`、`v-ripple`、`v-highlight`）
- 注册 Element Plus 全部图标
- 注册 vue-i18n 国际化
- 导入全局样式

**修改场景**：添加新的全局插件或全局组件时在此文件注册。

#### [`src/config/index.ts`](src/config/index.ts) — 系统主配置

项目的全局配置中心，包含：

- `systemInfo`：系统名称（`Art Design Pro`）
- `elementPlusTheme`：Element Plus 主题主色（默认 `#5D87FF`）
- `systemThemeStyles`：系统主题样式定义
- `settingThemeList`：主题设置列表（亮色/暗色/跟随系统）
- `menuLayoutList`：菜单布局列表（左侧/顶部/混合/双栏）
- `themeList`：菜单主题列表（Design/Dark/Light）
- `systemMainColor`：系统主题色选项（7 种可选颜色）
- `systemSetting`：默认菜单宽度 240px、默认圆角 0.75、默认标签样式
- `fastEnter` / `headerBar`：快速入口和顶栏功能配置

**修改场景**：修改系统名称、品牌主色、默认菜单宽度、默认布局等全局配置。

#### [`src/App.vue`](src/App.vue) — 根组件

处理 Element Plus 国际化语言切换、主题动画、存储兼容性检查、用户信息获取。

**修改场景**：添加全局初始化逻辑（如全局 websocket 连接、全局轮询等）。

---

### 4.2 路由与权限（核心架构）

项目的权限体系是二次开发中最重要的部分，理解它才能正确添加业务页面。

#### 权限模式

项目通过环境变量 `VITE_ACCESS_MODE` 支持两种权限模式：

| 模式 | `VITE_ACCESS_MODE` 值 | 菜单来源 | 权限过滤 |
| --- | --- | --- | --- |
| 前端控制（默认） | `frontend` | [`asyncRoutes.ts`](src/router/routes/asyncRoutes.ts) 静态配置 | 根据用户 `roles` 过滤 |
| 后端控制 | `backend` | 后端接口 `/api/menu/list` 返回 JSON | 后端已完成过滤 |

**权限模式判断**见 [`useCommon.ts`](src/composables/useCommon.ts:11-13)：

```typescript
const isFrontendMode = computed(() => {
  return import.meta.env.VITE_ACCESS_MODE === 'frontend'
})
```

#### 核心路由文件

| 文件 | 作用 | 关键说明 |
| --- | --- | --- |
| [`src/router/index.ts`](src/router/index.ts) | 路由实例创建（Hash 模式） | `HOME_PAGE_PATH` 可用于强制指定首页 |
| [`src/router/routes/staticRoutes.ts`](src/router/routes/staticRoutes.ts) | **静态路由**（无需登录/权限） | 登录、注册、忘记密码、404、500、iframe 外部页面 |
| [`src/router/routes/asyncRoutes.ts`](src/router/routes/asyncRoutes.ts) | **异步路由 = 菜单配置** | **新增业务页面的主文件** |
| [`src/router/routesAlias.ts`](src/router/routesAlias.ts) | 路由路径别名枚举 | 用于 `router.push(RoutesAlias.Login)` 跳转 |
| [`src/router/guards/beforeEach.ts`](src/router/guards/beforeEach.ts) | **路由守卫核心** | 登录校验、动态路由注册、菜单角色过滤 |

#### 路由 Meta 元数据说明

在异步路由中，每个路由的 `meta` 支持以下字段：

| 字段            | 类型         | 说明                                                      |
| --------------- | ------------ | --------------------------------------------------------- |
| `title`         | `string`     | 菜单标题（i18n key 或直接字符串）                         |
| `icon`          | `string`     | 菜单图标（Unicode 编码）                                  |
| `roles`         | `string[]`   | 角色权限（如 `['R_SUPER', 'R_ADMIN']`），前端权限模式生效 |
| `keepAlive`     | `boolean`    | 是否缓存页面组件                                          |
| `isHide`        | `boolean`    | 是否在菜单中隐藏                                          |
| `isHideTab`     | `boolean`    | 是否在标签页中隐藏                                        |
| `isFullPage`    | `boolean`    | 是否全屏显示                                              |
| `fixedTab`      | `boolean`    | 是否固定标签页（不可关闭）                                |
| `activePath`    | `string`     | 激活的菜单路径（用于详情页高亮父菜单）                    |
| `showBadge`     | `boolean`    | 是否显示角标                                              |
| `showTextBadge` | `string`     | 显示文字角标内容                                          |
| `authList`      | `AuthItem[]` | 按钮级权限列表（`{ title, authMark }`）                   |
| `link`          | `string`     | 外链地址                                                  |
| `isIframe`      | `boolean`    | 是否是 iframe 页面                                        |
| `setTheme`      | `boolean`    | 是否设置独立主题                                          |
| `noLogin`       | `boolean`    | 无需登录即可访问（静态路由专用）                          |

#### 路由守卫执行流程

```
用户访问 URL
  │
  ├─ 未登录 → 跳转登录页
  │
  ├─ 已登录、未注册动态路由
  │    ├─ 前端模式：从 asyncRoutes 读取菜单，按用户 roles 过滤
  │    └─ 后端模式：调用 /api/menu/list 获取菜单数据
  │    └─ 注册动态路由 → 重定向到目标页
  │
  ├─ 已登录、已注册路由 → setWorktab / setPageTitle → 正常放行
  │
  └─ 无匹配路由 → 跳转 404
```

---

### 4.3 状态管理 Store

| 文件 | 作用 | 持久化 | 修改场景 |
| --- | --- | --- | --- |
| [`src/store/modules/user.ts`](src/store/modules/user.ts) | 用户登录态、Token、语言、锁屏 | ✅ localStorage | 扩展用户信息字段、修改登录/登出行为 |
| [`src/store/modules/setting.ts`](src/store/modules/setting.ts) | 菜单类型/主题/布局/显示项开关 | ✅ localStorage | 修改默认设置值 |
| [`src/store/modules/menu.ts`](src/store/modules/menu.ts) | 菜单列表、首页路径、动态路由管理 | ❌ | 一般无需修改 |
| [`src/store/modules/worktab.ts`](src/store/modules/worktab.ts) | 已打开标签页列表 | ✅ localStorage | 修改标签页行为 |
| [`src/store/modules/table.ts`](src/store/modules/table.ts) | 表格列配置缓存 | ❌ | 修改表格列缓存策略 |

---

### 4.4 HTTP 请求层

[`src/utils/http/index.ts`](src/utils/http/index.ts) 是 Axios 的核心封装，包含：

- **请求拦截器**：自动注入 `Authorization` Token
- **响应拦截器**：根据 `code` 判断成功/未授权/错误
- **自动重试**：超时、500、502、503、504 自动重试最多 2 次
- **统一错误处理**：`HttpError` 类 + `showError` 提示

**对接后端时的重要检查点**：

1. **API 地址**：修改 [`.env.production`](.env.production) 中的 `VITE_API_URL`
2. **响应结构**：确保后端返回 `{ code, msg, data }` 结构，成功时 `code` 需与 [`status.ts`](src/utils/http/status.ts) 中的 `ApiStatus.success` 一致
3. **Token 传递**：请求头中 `Authorization` 字段的格式是否需要调整
4. **登录过期处理**：`ApiStatus.unauthorized` 对应的状态码是否需要修改

#### API 层示例（[`src/api/usersApi.ts`](src/api/usersApi.ts)）

```typescript
import request from '@/utils/http'

export class UserService {
  // GET 请求
  static getUserInfo() {
    return request.get<Api.User.UserInfo>({
      url: '/api/user/info'
    })
  }

  // POST 请求
  static login(params: Api.Auth.LoginParams) {
    return request.post<Api.Auth.LoginResponse>({
      url: '/api/auth/login',
      params
    })
  }

  // 带分页的列表请求
  static getUserList(params: Api.Common.PaginatingSearchParams) {
    return request.get<Api.User.UserListData>({
      url: '/api/user/list',
      params
    })
  }
}
```

---

## 五、二次开发实操指南

### 5.1 新增一个业务页面的完整流程

假设要新增一个"订单管理"页面。

#### 第一步：添加路由别名

在 [`src/router/routesAlias.ts`](src/router/routesAlias.ts) 中添加：

```typescript
export enum RoutesAlias {
  // ... 已有别名
  OrderList = '/order/list', // 订单列表
  OrderDetail = '/order/detail' // 订单详情
}
```

#### 第二步：添加异步路由（菜单配置）

在 [`src/router/routes/asyncRoutes.ts`](src/router/routes/asyncRoutes.ts) 中的 `asyncRoutes` 数组添加：

```typescript
{
  path: '/order',
  name: 'Order',
  component: RoutesAlias.Layout, // 使用主布局
  meta: {
    title: '订单管理',             // 菜单标题（也支持 i18n key）
    icon: '&#xe8d4;',            // 菜单图标 Unicode
    roles: ['R_SUPER', 'R_ADMIN'] // 前端权限：仅超管和管理员可访问
  },
  children: [
    {
      path: 'list',
      name: 'OrderList',
      component: RoutesAlias.OrderList,
      meta: {
        title: '订单列表',
        keepAlive: true,          // 缓存页面
        authList: [               // 按钮级权限
          { title: '新增', authMark: 'add' },
          { title: '编辑', authMark: 'edit' },
          { title: '删除', authMark: 'delete' }
        ]
      }
    },
    {
      path: 'detail',
      name: 'OrderDetail',
      component: RoutesAlias.OrderDetail,
      meta: {
        title: '订单详情',
        isHide: true,             // 不在菜单中显示
        activePath: '/order/list' // 激活父菜单高亮
      }
    }
  ]
}
```

#### 第三步：创建页面组件

创建 [`src/views/order/list/index.vue`](src/views/order/list/index.vue) 和 [`src/views/order/detail/index.vue`](src/views/order/detail/index.vue)。

可参考现有页面：

- **CRUD 表格** → 参考 [`src/views/system/user/index.vue`](src/views/system/user/index.vue)
- **图表分析** → 参考 [`src/views/dashboard/analysis/index.vue`](src/views/dashboard/analysis/index.vue)

#### 第四步：创建 API 接口

在 [`src/api/`](src/api/) 下新建 [`orderApi.ts`](src/api/orderApi.ts)：

```typescript
import request from '@/utils/http'

export class OrderService {
  static getOrderList(params: any) {
    return request.get({
      url: '/api/order/list',
      params
    })
  }

  static getOrderDetail(id: string) {
    return request.get({
      url: `/api/order/detail/${id}`
    })
  }
}
```

#### 第五步：添加国际化文本（可选）

在 [`src/locales/langs/zh.json`](src/locales/langs/zh.json) 和 [`en.json`](src/locales/langs/en.json) 中添加菜单翻译。

---

### 5.2 对接后端 API

1. **修改环境变量**：[`.env.development`](.env.development) / [`.env.production`](.env.production) 中的 `VITE_API_URL`
2. **检查响应数据结构**：[`src/utils/http/index.ts:65-78`](src/utils/http/index.ts:65-78) 中期望 `{ code, msg, data }` 结构
3. **检查 Token 传递**：[`src/utils/http/index.ts:47-53`](src/utils/http/index.ts:47-53) 中 Token 通过 `Authorization` 请求头传递
4. **对接菜单接口**（后端权限模式）：修改 [`src/api/menuApi.ts`](src/api/menuApi.ts) 中的 `getMenuList()` 为真实请求

---

### 5.3 修改登录页/认证流程

- 登录页面：[`src/views/auth/login/index.vue`](src/views/auth/login/index.vue)
- 登录逻辑（Composable）：[`src/composables/useAuth.ts`](src/composables/useAuth.ts)
- 用户状态：[`src/store/modules/user.ts`](src/store/modules/user.ts)

---

### 5.4 修改主题/布局默认值

在 [`src/config/index.ts`](src/config/index.ts) 中修改：

| 配置项       | 路径                                | 说明                   |
| ------------ | ----------------------------------- | ---------------------- |
| 系统名称     | `systemInfo.name`                   | 浏览器标签页标题       |
| 品牌主色     | `elementPlusTheme.primary`          | 按钮/链接/高亮等主色   |
| 默认菜单宽度 | `systemSetting.defaultMenuWidth`    | 侧边栏宽度（默认 240） |
| 默认圆角     | `systemSetting.defaultCustomRadius` | 卡片/按钮圆角大小      |
| 默认标签样式 | `systemSetting.defaultTabStyle`     | 标签页样式             |

---

### 5.5 按钮级权限控制

在页面中使用 `v-auth` 指令控制按钮显示：

```vue
<el-button v-auth="'add'">新增</el-button>
<el-button v-auth="'edit'">编辑</el-button>
<el-button v-auth="'delete'">删除</el-button>
```

前提是在路由 meta 中配置了对应的 `authList`：

```typescript
meta: {
  authList: [
    { title: '新增', authMark: 'add' },
    { title: '编辑', authMark: 'edit' },
    { title: '删除', authMark: 'delete' }
  ]
}
```

---

## 六、二次开发重点关注文件清单（优先级排序）

| 优先级 | 文件 | 修改场景 |
| --- | --- | --- |
| ⭐⭐⭐⭐⭐ | [`src/router/routes/asyncRoutes.ts`](src/router/routes/asyncRoutes.ts) | **每次新增业务页面必须修改** |
| ⭐⭐⭐⭐⭐ | [`.env.development`](.env.development) / [`.env.production`](.env.production) | 对接后端时修改 API 地址 |
| ⭐⭐⭐⭐ | [`src/router/routesAlias.ts`](src/router/routesAlias.ts) | 新增页面时添加路由别名 |
| ⭐⭐⭐⭐ | [`src/utils/http/index.ts`](src/utils/http/index.ts) | 对接后端时检查响应结构匹配 |
| ⭐⭐⭐⭐ | [`src/config/index.ts`](src/config/index.ts) | 修改系统名称/品牌色/默认布局 |
| ⭐⭐⭐ | [`src/store/modules/user.ts`](src/store/modules/user.ts) | 修改用户信息字段/登录登出逻辑 |
| ⭐⭐⭐ | [`src/router/guards/beforeEach.ts`](src/router/guards/beforeEach.ts) | 修改权限/登录路由行为 |
| ⭐⭐⭐ | [`src/api/usersApi.ts`](src/api/usersApi.ts) | API 层参考模板 |
| ⭐⭐⭐ | [`src/locales/langs/zh.json`](src/locales/langs/zh.json) / [`en.json`](src/locales/langs/en.json) | 国际化文本 |
| ⭐⭐⭐ | [`src/composables/useAuth.ts`](src/composables/useAuth.ts) | 修改登录流程 |
| ⭐⭐ | [`src/views/index/index.vue`](src/views/index/index.vue) | 修改主布局结构 |
| ⭐⭐ | [`src/views/system/user/index.vue`](src/views/system/user/index.vue) | CRUD 表格的标准写法参考 |
| ⭐⭐ | [`src/store/modules/setting.ts`](src/store/modules/setting.ts) | 修改系统默认设置值 |
| ⭐ | [`src/views/auth/login/index.vue`](src/views/auth/login/index.vue) | 修改登录页面 UI |
| ⭐ | [`src/views/dashboard/`](src/views/dashboard/) | 修改仪表盘首页 |

---

## 七、常见问题

### Q1：项目启动报错 `pnpm: command not found`？

系统未安装 pnpm，执行：`npm install -g pnpm`

### Q2：登录后菜单为空？

检查 `.env` 中 `VITE_ACCESS_MODE` 的值：

- `frontend`：检查 [`asyncRoutes.ts`](src/router/routes/asyncRoutes.ts) 中是否配置了对应角色的 `roles`
- `backend`：检查 API 返回的菜单数据是否正确

### Q3：页面新增后左侧菜单不显示？

确认以下几点：

1. [`RoutesAlias`](src/router/routesAlias.ts) 中已添加路由别名
2. [`asyncRoutes.ts`](src/router/routes/asyncRoutes.ts) 中 `component` 指向正确
3. 页面组件文件路径与实际路径一致
4. 用户角色拥有该路由的 `roles` 权限

### Q4：如何去掉某个默认菜单？

在 [`asyncRoutes.ts`](src/router/routes/asyncRoutes.ts) 中删除对应路由对象，或为其 `meta` 添加 `isHide: true`。

### Q5：如何自定义 404 页面？

修改 [`src/views/exception/404/index.vue`](src/views/exception/404/index.vue)。

---

> 更多详情请查阅 [官方文档](https://www.lingchen.kim/art-design-pro/docs/)
