

## Nextjs 中的特殊组件或约定

- `layout.tsx`: 
- `page.tsx`: 
- `loading.tsx`: 一个基于 React `Suspense` 构建的特殊 Next.js 文件。它使得在页面内容加载期间作为替代页面显示出来。在 `loading.tsx` 中添加的任何 UI 元素都将作为静态文件的一部分，并在页面加载时发送给客户端。

- `()`: `()` 括号创建了一个路由组 (Route Groups)，它允许将文件按逻辑组进行整理，同时不影响 URL 路径结构。当用括号 `()` 创建新文件夹时，文件夹名称不会包含在 URL 路径中。因此，`/dashboard/(overview)/page.tsx` 将依然对应 `/dashboard` URL。

## 客户端组件

Next.js 默认是服务端组件，可以在组件顶部编写 "use client" 来指示该组件是纯客户端运行的组件。

```typescript
"use client"

```

客户端组件与传统 SPA 程序组件的功能一样，可以访问 DOM API，使用事件处理器及 hooks。

## 局部预渲染 (PPR - Partial Prerendering)

Next.js 14 引入的一种全新的渲染模型，能够在同一路由下同时利用静态渲染和动态渲染的双重优势。PPR 利用 React 的 `Suspense` 组件让应用程序的部分内容延迟，直到满足某些条件 (例如数据加载完成) 时再进行渲染。可以将动态组件包裹在 `Suspense` 组件中，并为它传递一个 fallback 组件，以便在动态组件加载期间显示该组件。传递给 `fallback` 的组件会同其他静态元素一起嵌入到初始 HTML 文件中，在构建时 (或重新验证时) 静态内容先行预渲染为一个壳，并为动态内容预留占位符。动态内容直到用户访问该路由时才开始渲染。

```typescript
<Suspense fallback={<RevenueChartSkeleton />}>
  <RevenueChart />
</Suspense>
```

将一个组件包裹在 `Suspense` 中并不会使该组件本身变得具有动态性，而是将 `Suspense` 视作静态代码和动态代码之间的分界线。

### 启用 PPR 功能

要在 Next.js 项目中启用 PPR，打开 `next.config.ts` 文件:

```typescript
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  experimental: {
    ppr: "incremental",
  },
};

export default nextConfig;
```

之后，在需要启用 PPR 的路由对应的 `layout.tsx` 文件中添加一行代码:

```typescript
export const experimental_ppr = true;
```