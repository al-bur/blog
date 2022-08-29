---
title: import 자동 정렬 (여러분의 import문 안녕하신가요?)
date: 2022-06-10
category: 기타
thumbnail: { thumbnailSrc }
draft: false
---

## 글을 작성하게 된 배경

- 미션을 진행하면서 항상 느끼는 것이 수많은 import문을 매번 수작업으로 정리하는데 시간이 너무 많이 걸린다는 것이었습니다.
- import문을 정렬하지 않고 개발을 진행한다면 아래처럼 될 겁니다.

```javascript
import { ReactComponent as Cart } from 'assets/cart.svg'
import Header from 'components/@shared/Header/Header'
import NavigateButton from 'components/@shared/NavigateButton/NavigateButton'
import PageTitle from 'components/@shared/PageTitle/PageTitle'
import useClickCartButton from 'hooks/useClickCartButton'
import ProductDetailPage from 'pages/ProductDetailPage/ProductDetailPage'
import ShoppingCartPage from 'pages/ShoppingCartPage/ShoppingCartPage'
import React, { useState } from 'react'
import { Routes, Route, useNavigate } from 'react-router-dom'
import { fetchCartsError } from 'redux/carts/carts.action'
import styled from 'styled-components'
```

- 또한, 미션에서 정한 자기만의 import문 정렬에 대한 컨벤션이 있다면, 해당 컨벤션을 지켜가면서, 또 까먹으면 이전에 만들었던 컴포넌트에 가서 import를 어떻게 정렬했는지 참고해서 정렬해주어야했습니다.
- 이렇다 보니, 매번 수동으로 정렬하는 import를 자동으로 정렬해주는 방법을 찾게 되었습니다.

## 방법 (prettier 사용)

- import를 해줄 수 있는 방법을 여러개 찾을 수 있었습니다.

  - eslint 활용
  - prettier 활용
  - etc

<br>

- 이 중에서 `prettier`와 [prettier-plugin-sort-imports](https://github.com/trivago/prettier-plugin-sort-imports) 라이브러리를 활용하여 import문을 자동 정렬해주었습니다.

```bash
npm install --save-dev @trivago/prettier-plugin-sort-imports
```

설치가 완료되었으면, prettier config 파일에 아래처럼 정렬 순서를 설정해주면 됩니다 (정규식 사용😀)

```json
// prettierrc.json
{
  "importOrder": [
    "^(@|pages)(.*|$)",
    "^(@|components/@shared)(.*|$)",
    "^(@|components)(.*|$)",
    "^(@|redux/)(.*|$)",
    "^(@|hooks)(.*|$)",
    "^(@|styles)(.*|$)",
    "^(@|(assets|constants|utils|api))(.*|$)"
  ],
  "importOrderSeparation": true
}
```

## 결과

```javascript
import React, { useState } from 'react'
import { Routes, Route, useNavigate } from 'react-router-dom'
import styled from 'styled-components'

import ProductDetailPage from 'pages/ProductDetailPage/ProductDetailPage'
import ShoppingCartPage from 'pages/ShoppingCartPage/ShoppingCartPage'

import Header from 'components/@shared/Header/Header'
import NavigateButton from 'components/@shared/NavigateButton/NavigateButton'
import PageTitle from 'components/@shared/PageTitle/PageTitle'

import { fetchCartsError } from 'redux/carts/carts.action'

import useClickCartButton from 'hooks/useClickCartButton'

import { ReactComponent as Cart } from 'assets/cart.svg'
```

## 근데, 일일이 파일에 들어가서 save를 해주면서 적용해줘야하나?

```json
// package.json
scripts: {
"pretty": "prettier --write \"src/\*_/_.{js,jsx,json}\""
}
```

```bash
npm run pretty
```

위의 명령어를 사용해서 전체 파일에 적용을 해줄 수 있습니다. `\"src/**/*.{js,jsx,json}\"`은 경로내 파일들을 지칭해주기 때문에 커스터마이징 해주면 됩니다.
