---
title: github action 활용해서 build후 github page 배포하기
date: 2022-02-24 11:23:00
category: 기타
thumbnail: { thumbnailSrc }
draft: false
---

자동차 경주 미션에서 미션을 다 끝낸 후 웹사이트를 사용해볼 수 있도록 github에서 제공하는 github page 기능을 이용해 웹 사이트를 배포하였습니다. 이번 로또 미션에도 똑같이 웹 사이트를 배포하였습니다.

그런데, 이번 미션에서는 webpack을 활용해서 build를 해주도록 하였기 때문에 index.html에 script 태그를 넣지 않았습니다. 그러다보니 `기본적인 github page 기능을 사용하면 html만 덩그러니 배포되는 것을 알게 되었습니다.` 검색을 해보니 자동으로 build 후 배포가 되도록 해줘야한다는 것입니다.

검색 결과, `github workflow`를 이용해서 배포 전에 스스로 build를 할 수 있도록 설정해주어야 했습니다.

## github action을 활용하여 build후 배포하기

1. 먼저, 작업물 branch 최상단 루트에 .github/workflows라는 디렉토리를 만듭니다. 그리고 하위에 `main.yml(파일명은 아무거나 상관없습니다)을 하나 만들어줍니다.` 이 파일은 설명서라고 보시면 됩니다.

<br>

2. yml 파일에 workflow를 작성해주면 됩니다.

```yml
name: Build and GH-Page Deploy

on:
  push:
    branches:
      - practice

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          persist-credentials: false

      - name: Install
        run: npm install

      - name: Build
        run: |
          npm run build

      - name: Deploy to GH Page
        uses: JamesIves/github-pages-deploy-action@4.1.1
        with:
          branch: deploy
          folder: dist
```

하나하나 설명해보자면 먼저 `practice라는 branch에 push가 감지되면 jobs`를 실행시켜준다라고 보면 됩니다.

이후 checkout(practice 레포에서 가장 최신의 코드를 클론한다), install(dependencies 설치한다), Build(빌드한다), Deploy(dist 폴더에 빌드한 파일을 deploy라는 브랜치에 푸시한다)를 따르도록 설정해주었습니다.

<br>

3. 이렇게 하면 deploy라는 branch가 생성되고 settings에서 deploy branch를 사용하여 배포하라고 설정해주면 정상적으로 배포가 됩니다.
