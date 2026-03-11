---
title: Next.js 开发笔记
author: Frost He
excerpt: 记录 Next.js 框架的核心概念、路由系统、组件模式和最佳实践
date: 2025-03-07
category:
- WebFrontend
tag:
- nextjs
- react
- routing
---

# Next.js 开发笔记

这篇笔记以 Next.js App Router 为主线，按「路由与文件约定 → 组件模型 → 错误处理 → 渲染模型 → React 新特性」的顺序整理。

## 1. 路由与文件约定（App Router）

### 1.1 路由分组（Route Groups）

`()`：括号用于创建路由分组（Route Groups）。它可以把文件按逻辑分组整理，但不会影响 URL 结构。

例如：`/dashboard/(overview)/page.tsx` 仍然对应 `/dashboard`。

详情： [🌐 Route Groups](https://nextjs.org/docs/app/api-reference/file-conventions/route-groups)

### 1.2 动态路由（Dynamic Route Segments）

`[]`：用于定义动态路由参数，例如 `[id]`、`[post]`、`[slug]`。

中括号内的值会作为 route token 的 key，最终以 `params` 的形式传入 `page.tsx`（或 `layout.tsx`）对应的组件。

详情： [🌐 Dynamic Route Segments](https://nextjs.org/docs/app/api-reference/file-conventions/dynamic-routes)

### 1.3 根目录约定文件（安全与请求拦截）

- `auth.config.ts`：必须位于项目根目录，用于配置 [🌐 next-auth.js](https://authjs.dev/reference/nextjs)（认证 / 授权）。
- `proxy.ts`：必须位于项目根目录，用于编写请求发送前后的拦截逻辑，类似传统中间件。
  - 详情： [🌐 proxy.ts](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)

### 1.4 约定注意项

- `redirect()` 基于抛出 `Error` 实现；如果你在 `try/catch` 中使用它，不要把它放进 `try` 块里，否则会被误捕获。

参考： [🌐 Next.js Templates](https://vercel.com/templates)

## 2. 特殊文件与组件类型

### 2.1 特殊文件（File Conventions）

- `layout.tsx`：布局组件。
- `page.tsx`：页面组件。
- `loading.tsx`：基于 React `Suspense` 的加载占位 UI。`loading.tsx` 的 UI 会作为静态资源的一部分在页面加载时发送到客户端。
- `error.tsx`：路由级错误边界（捕获该路由下的子路由渲染错误）。
- `not-found.tsx`：处理 404 / `notFound()` 场景；优先级高于 `error.tsx`。
- `global-error.tsx`：全局错误处理组件，放在根路由下；必须包含 `<html>` 与 `<body>`，因为它会替换根 `layout` 的模板。

### 2.2 内置组件

- `Link`
- `Suspense`

### 2.3 Client Component（"use client"）

Next.js 默认使用 Server Component。若要声明某个组件在客户端运行，需要在文件顶部添加 `"use client"`：

```ts
"use client";
```

Client Component 与传统 SPA 组件类似：可以访问 DOM API、使用事件处理器与 React Hooks。

## 3. 错误处理

### 3.1 `error.tsx`：通用异常处理

文档： [🌐 error](https://nextjs.org/docs/app/api-reference/file-conventions/error)

`error.tsx` 是路由级错误边界（捕获所有子路由的渲染错误），有几个关键点：

- `error.tsx` 必须是 Client Component（即包含 `"use client"`）。
- 组件会接收两个由框架注入的参数：
  - `error`：原生 `Error` 实例。
  - `reset()`：重置错误边界并尝试重新渲染该路由。

> 注意：错误边界不会捕获事件处理器中的错误（它们发生在渲染之后）。这类错误需要你在业务逻辑中自行处理，并在合适时机展示友好 UI。

样板代码：

```tsx
"use client";

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <main className="flex h-full flex-col items-center justify-center">
      <h2 className="text-center">Something went wrong!</h2>
      <button
        className="mt-4 rounded-md bg-blue-500 px-4 py-2 text-sm text-white transition-colors hover:bg-blue-400"
        onClick={() => reset()}
      >
        Try again
      </button>
    </main>
  );
}
```

### 3.2 `not-found.tsx`：特定错误场景

文档： [🌐 not-found](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)

- Next.js 提供 `notFound()`（`next/navigation`）用于触发 404。
- 在对应路由目录下创建 `not-found.tsx`，用于渲染 404 UI。

## 4. 渲染模型：PPR（Partial Prerendering）

PPR（局部预渲染）是 Next.js 14 引入的渲染模型：同一路由下同时利用静态渲染与动态渲染。

核心思路：用 React `Suspense` 把「静态壳」与「动态内容」分隔开。

```tsx
<Suspense fallback={<RevenueChartSkeleton />}>
  <RevenueChart />
</Suspense>
```

注意：把组件包在 `Suspense` 里并不会让组件本身“变动态”，`Suspense` 更像是静态与动态的边界。

### 4.1 启用 PPR

1）在 `next.config.ts` 中开启实验特性：

```ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  experimental: {
    ppr: "incremental",
  },
};

export default nextConfig;
```

2）在需要启用 PPR 的路由 `layout.tsx` 中声明：

```ts
export const experimental_ppr = true;
```

## 5. Server Component vs Client Component

参考： [🌐 Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

### 5.1 Server Component 适合做什么

- 数据获取：Database、API 请求等。
- 需要保密的信息：API keys、tokens。
- 减少发送到客户端的 JavaScript 代码。
- 读取 `cookies` / `headers` / `session`。

### 5.2 Client Component 适合做什么

- 本地状态。
- 事件处理：`onClick`、`onChange` 等。
- 生命周期逻辑：`useEffect`、`useState` 等。
- 调用浏览器专属 API：`window`、`localStorage`、`Navigator.geolocation` 等。
- 自定义 Hooks。

> Client Component 使用 `"use client"` 指令后，它的所有 imports 与子组件都会被打包进客户端 bundle。

一个常见模式：在 Client Component 中用 `children` 作为 slot，传入 Server Component。

## 6. React 新特性（Next.js 依赖的能力）

### 6.1 React Server Components

文档： [🌐 Server Components](https://react.dev/reference/rsc/server-components)

### 6.2 React Server Actions

React Server Actions 允许你直接在服务器上运行异步代码，而不必额外创建 API endpoint 来修改数据。

它针对常见 Web 安全场景提供了多种机制（例如 encrypted closures、严格输入检查、错误消息哈希、host restrictions 等），以提升整体安全性。

文档： [🌐 Server Functions](https://react.dev/reference/rsc/server-functions)

#### 6.2.1 Server Actions 与 `useActionState`

文档： [🌐 useActionState](https://react.dev/reference/react/useActionState)

`useActionState(action, initialState, permalink?)` 是一个基于表单提交来更新状态的 Hook：

- `action(prevState, ...)`：表单提交时调用；第一个参数是 `prevState`。
- `initialState`：初始状态。
- `permalink`：提交后导航到的 URL（可选）。

返回值：

- `state`：当前状态。
- `formAction`：可传给 `<form action={...}>` 的 action 函数，或在 `startTransition` 中调用。
- `isPending`：是否正在提交。