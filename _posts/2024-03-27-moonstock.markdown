---
layout: post
title: "숫자만 다루더라도 Input element의 type을 number로 하면 안 되는 이유"
date: 2024-03-27 10:28:07 +0900
tags: moonstock
---

Input element의 type을 number로 할 경우 string의 입력을 막는 등 기본적인 조치가 취해진다.

하지만 이 number를 조금이라도 가공할 생각이라면, number type으로 하는 게 오히려 불편해진다.

### 입력 후 값을 지우더라도 빈칸이 안 된다

숫자 값을 입력한 다음 그 킨을 비우고 싶어서 백스페이스로 지우더라도 다시 빈칸으로 돌아갈 수 없다. 숫자 형태기 때문에 '0'값이 그대로 남는 것이다.

그래서 이 부분을 처리해 주기 위해서라도 타입을 number|""로 지정해 줘야 한다. 그리고 빈칸에 대한 처리를 따로 해 줘야 한다.

input에 있는 값으로 연산이라도 하려면, 값이 ""가 아닌지를 늘 체크해 줘야 한다.

### 가독성을 위해 숫자에 쉼표로 단위 구분을 할 경우 결국 string이 된다

큰 숫자는 가독성을 위해 세 자리마다 쉼표를 찍어주곤 한다. 자바스크립트에서는 이를 쉽게 구현할 수 있는 방법을 제공한다.

```ts
const formatNumber = (number: number) => {
  return new Intl.NumberFormat("ko-kr").format(number);
};
```

그런데 여기에서 return해 주는 값은 string 타입이다. 빈칸 처리를 위해서 input의 타입을 `number|""`로 해 주었다고 하더라도, 숫자에 쉼표가 달리는 순간 `string`도 다루어줘야 하는 것이다.

소수점 자리수를 잘라서 보여주는 `toFixed()` 같은 경우도 `string` 타입이 되기 때문에 소수점이 생기는 숫자를 다루려면 매개변수의 타입은 `string`까지 취급해 줘야 한다.

```ts
export const formatNumberToKorean = (number: number | string) => {
  const numType = typeof number === "string" ? parseFloat(number) : number;
  return isNaN(numType) ? "" : new Intl.NumberFormat("ko-kr").format(numType);
};
```
