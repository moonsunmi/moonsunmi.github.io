---
layout: post
title: "Next.js와 styled-component 사용하기"
date: 2024-03-12 12:18:07 +0900
tags: moonstock
---

###### moonstock 앱을 만들면서 겪은 경험에 대한 기록입니다.

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

#### 4. \_document.tsx 만들기

`pages/_document.tsx`에 다음과 같은 코드를 추가한다. `_document.tsx`는 애플리케이션의 페이지를 구성하는 기본 템플릿 역할을 한다.

```js
import type { DocumentContext, DocumentInitialProps } from "next/document";
import Document from "next/document";
import { ServerStyleSheet } from "styled-components";

export default class MyDocument extends Document {
  static async getInitialProps(
    ctx: DocumentContext
  ): Promise<DocumentInitialProps> {
    const sheet = new ServerStyleSheet();
    const originalRenderPage = ctx.renderPage;

    try {
      ctx.renderPage = () =>
        originalRenderPage({
          enhanceApp: (App) => (props) =>
            sheet.collectStyles(<App {...props} />),
        });

      const initialProps = await Document.getInitialProps(ctx);
      return {
        ...initialProps,
        styles: (
          <>
            {initialProps.styles}
            {sheet.getStyleElement()}
          </>
        ),
      };
    } finally {
      sheet.seal();
    }
  }
}
```

### \_document.tsx

그렇다면 `_document.tsx`는 무엇일까? Next.js에서 `_document.tsx`는 주로 애플리케이션의 HTML 문서 구조를 정의하기 위해 사용한다.

웹 폰트를 로드하거나, <html>, <body> 태그에 공통 속성을 추가하는 등의 작업을 할 수 있다.

이 파일은 서버에서 단 한 번만 로드되며, 클라이언트에서는 사용되지 않는다.

이 문서를 통해서 전체 HTML 문서 구조를 커스터마이즈할 수 있는데, 이를 활용해 서버사이트렌더링, 클라이언트사이드 렌더링 간의 스타일 불일치 문제를 해결할 수 있다.

`ServerStyleSheet`는 서버사이드렌더링에서 생성된 스타일을 수집하는데, 이를 HTML 문서에 포함시켜 클라이언트로 전달한다.

이렇게 하면, 클라이언트사이드에서 로드한 스타일과 서버 사이드에서 렌더링한 스타일이 같아진다.

### \_document.tsx와 registry.tsx의 역할

서버사이드렌더링과 클라이언트사이드렌더링 사이의 불일치를 해결하려는 목적을 가졌다는 공통점이 있지만, 서로 다른 역할을 수행한다. 둘 다 서버에서 실행되고, 결과를 클라이언트에 전달한다.

\_document.tsx는 HTML 문서 구조를 정의한다. 전체 웹 페이지의 구조를 설정한다. 특히 서버사이드렌더링을 통해 초기 페이지 로드 시 중요한 역할을 한다. 이 파일은 애플리케이션에서 초기 한 번만 로딩되기 때문에 전역 설정에 관여하면 좋다.

registry.tsx는 스타일 시트 레지스트리 역할을 한다. 스타일 관리에 좀 더 초점을 둔다. 서버와 클라이언트 사이의 스타일에 일관성을 유지하는 데 필수적이다.
