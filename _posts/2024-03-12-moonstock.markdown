---
layout: post
title: "Next.js와 styled-component 사용하기"
date: 2024-03-12 12:18:07 +0900
tags: moonstock
---

Next.js 프로젝트에서는 styled-component와 같은 CSS-in-JS 라이브러리를 사용하려면, 서버사이드렌더링에서 생성된 스타일을 클라이언트 사이드로 옮겨주기 위한 설정이 필요하다.

이 작업을 해 주지 않으면 새로고침 시 스타일이 사라지는 부작용이 나타날 수 있다.

#### 1. nextConfig 설정하기

다음과 같이 next.config.js 안에서 styled-component를 사용 가능하게 설정한다.

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
  compiler: {
    styledComponents: true,
  },
};

export default nextConfig;
```

#### 2. lib/registry.tsx 파일 생성하기

`lib/registry.tsx` 파일을 다음과 같이 생성한다.

```js
"use client";

import React, { useState } from "react";
import { useServerInsertedHTML } from "next/navigation";
import { ServerStyleSheet, StyleSheetManager } from "styled-components";

export default function StyledComponentsRegistry({
  children,
}: {
  children: React.ReactNode,
}) {
  // Only create stylesheet once with lazy initial state
  // x-ref: https://reactjs.org/docs/hooks-reference.html#lazy-initial-state
  const [styledComponentsStyleSheet] = useState(() => new ServerStyleSheet());

  useServerInsertedHTML(() => {
    const styles = styledComponentsStyleSheet.getStyleElement();
    styledComponentsStyleSheet.instance.clearTag();
    return <>{styles}</>;
  });

  if (typeof window !== "undefined") return <>{children}</>;

  return (
    <StyleSheetManager sheet={styledComponentsStyleSheet.instance}>
      {children}
    </StyleSheetManager>
  );
}
```

`registry.tsx` 파일은 styled-component로 생성된 스타일을 수집한다.

#### 3. useServerInsertedHTML 훅

서버사이드 렌더링으로 실행된 HTML에 앞에서 모은 스타일을 입힌다. app/layout.tsx

```js
import StyledComponentsRegistry from "./lib/registry";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html>
      <body>
        <StyledComponentsRegistry>{children}</StyledComponentsRegistry>
      </body>
    </html>
  );
}
```

루트 레이아웃의 `children`을 `StyledComponentsRegistry` 컴포넌트로 감싸면 된다.
