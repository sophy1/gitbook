# Mono repo 사용 후기

### 도입 이유

* 재사용하고자 하는 ui component repository를 모으기 위해 Git submodule 보다 쉬운 방법인 모노레포 발견
* &#x20;모노레포: Git에서 두 개 이상의 프로젝트 코드가 동일한 repo에 저장되어, 서로 다른 프로젝트들을 관리하는 개발 전략

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

### &#x20;Pros & Cons 를 살펴보고, 얼마나 좋아지는지 계산해본다.

#### Pros

* 모노레포에서는 아래 작업을 한 번만 수행해도 되기 때문에, 프로젝트 생성 설정이 더 쉬움
  * 저장소 생성 → 사용자 권한 추가→ 개발환경 구축 → CI/CD 구축 → 빌드 → 배포
* 프로젝트 의존성(module dependency) 관리 쉬움
  * 패키지 버전 변경되더라도 멀티레포에서 모두 다 확인해야 하지만, 모노레포에서는 하나의 repo 안에서 확인하면 됨
  * 중복 module 설치 시간 단축 및 node\_modules 용량 감소
* 서로 코드 변화를 쉽게 파악할 수 있음
  * Commit할 때마다 변경 사항을 확인할 수 있음
* 일관된 개발자 경험 제공
  * 프로젝트를 일관되게 구축하고 테스트 가능. 예를 들면 prettier, eslint 룰 설정..

#### Cons

* 의존성 연결이 쉽기 때문에 과도한 의존 관계가 나타날 수 있음
* 도구들에 대한 레퍼런스가 많지 않음
* 만약 A, B 프로젝트가 mono repo 안에 있다면, CI, CD 구성이 같은 환경이라, 불필요한 배포가 발생할 수 있다.
  * A 프로젝트 develop branch에 소스코드 commit이 올라가면, B 프로젝트도 빌드 배포가 발생함. 점점 규모가 커질 수록 이에 대한 시간 비용이 많이 들 것으로 보임



### 어떤 도구를 사용하면 좋을지 찾아보자.

어떤 도구가 있는지 보자.

<figure><img src="../.gitbook/assets/image (7).png" alt="https://d2.naver.com/helloworld/7553804"><figcaption><p>출처: <a href="https://d2.naver.com/helloworld/7553804">https://d2.naver.com/helloworld/7553804</a></p></figcaption></figure>

기존 npm 과 사용 방법이 유사하고 단순 코드 공유 수준의 관리만 필요했기 때문에 yarn workspace 선택

#### [&#xD; ](https://d2.naver.com/helloworld/7553804)어떤 도구를 많이 사용하나?

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>출처: <a href="https://2021.stateofjs.com/en-US/libraries/monorepo-tools">https</a><a href="https://2021.stateofjs.com/en-US/libraries/monorepo-tools">://</a><a href="https://2021.stateofjs.com/en-US/libraries/monorepo-tools">2021.stateofjs.com/en-US/libraries/monorepo-tools</a></p></figcaption></figure>



#### 다른 mono repo는 어떤 도구를 사용하나?

* 간단한 boilerplate 프로젝트: https://github.com/slanatech/vue-monorepo-boilerplate
* Yarn: React, React-router, Babel (Yarn Berry)
* Lerna + Yarn: Next.js, Babel (v7.12.12), Jest, Create React App, Storybook Vue-cli, Nuxt.js, Webpack-cli, https://github.com/mui/material-ui
* Lerna + Npm: Apollo-server
* Nx: Storybook, FluentUI, NgRx
* Turborepo: Vercel, Lattice, TeeSpring, MakeSwift, On Deck, Astro
* Pnpm: Vue 3

#### yarn workspace

yarn workspace 에서 유령 dependency가 있어서 프로젝트 개발시 특정 package를 못찾는 문제가 발생할 수 있다. 이렇게 hoisting 안 해야 하는 경우, root directory의 package.json에서 nohoist 설정 작성할 수 있다.&#x20;

* 참고 링크1: https://classic.yarnpkg.com/blog/2018/02/15/nohoist
* 참고 링크2: [https://toss.tech/article/node-modules-and-yarn-berry](https://toss.tech/article/node-modules-and-yarn-berry)

```json
{
  "name": "mono-repo",
  "private": true,
  "version": "1.0.0",
  "license": "MIT",
  "workspaces": {
    "packages": [
      "packages/**"
    ],
    "nohoist": [
      "**/react-router-dom",
      "**/react-router-dom/**"
    ]
  }
}
```



### yarn workspace로 기본 구조 잡기

```shell
$ cd mono-test
$ npm install yarn -global

$ mkdir common component client

# component가 common 패키지 의존하게 하려면 component/package.json dependeces 에 common 추가 하거나 아래 명령 이용
$ yarn workspace component add common@1.0.0
$ yarn workspace client add common@1.0.0
$ yarn workspace client add component@1.0.0

# yarn workspace <WORKSPACE_NAME> <COMMAND_NAME> 
$ yarn workpspace client run build:dev
$ yarn workspace client run start:dev

# workspace 의존 관계 확인 (workspaces !)
$ yarn workspaces info 

# 모든 workspaces 에 대한 명령 실행
$ yarn workspaces run test
```

* 초기 workspace 생성시 cmd 실행하는 것보다 직접 pacakge.json의 dependencies 수정하는게 더 빠르다. cmd 로 자동으로 작성된 package.json에는 불필요한 설정 값도 넣어줘야 해서
* client, component 둘 다 같은 UI framework를 사용한다면, 주요 package (Vue.js나 React, core-js, vuex, saas-loader 버전)을 통일하는 게 유지보수에 좋다.
* 아래는 client workspace의 package.json 이다. dependencies에 common과 components를 추가하고, 빌드하면 common workspace와 components workspace가 root directory/node\_modules에 symbolic link로 생성된다.&#x20;
* 모든 workspace는 webpack 번들러를 사용했는데, client 빌드시 components 와 동일한 alias로 인해 components를 못 찾는 문제가 발생해서, client와 components의 alias를 서로 다르게 작성하여 해결했었다.&#x20;

```json
{
  "name": "client",
  "version": "1.0.0",
  "descripton": "Service Portal App client",
  "main": "src/index.js",
  "scripts": {
    "clean": "rm -rf dist",
    "start:dev": "webpack serve --mode development",
    "make:deps": "npm --prefix ../component run build:clientDeps",
    "serve:dev": "vite --mode dev",
    "debug:dev": "vite --mode dev --debug",
    "serve:prod": "vite --mode prod --debug",
    "build:dev": "npm run clean && npm run make:deps && cross-env NODE_OPTIONS=--max_old_space_size=4096 vite build --mode dev",
    "build:prod": "npm run clean && npm run make:deps && cross-env NODE_OPTIONS=--max_old_space_size=4096 vite build --mode prod",
    "preview:dev": "vite preview --mode dev",
    "preview:prod": "vite preview --mode prod",
    "lint": "eslint src",
    "test": "echo 'TEST TODO'"
  },
  "dependencies": {
    "common": "1.0.0",
    "components": "1.0.0",
    ...
  }
```



### Ref

* 모노레포 도구 설명: [https://d2.naver.com/helloworld/7553804](https://d2.naver.com/helloworld/7553804)
* 모노레포 개념 설명: [https://d2.naver.com/helloworld/0923884](https://d2.naver.com/helloworld/0923884)
* nohoist 설정: [https://classic.yarnpkg.com/blog/2018/02/15/nohoist/](https://classic.yarnpkg.com/blog/2018/02/15/nohoist/)
* 공식 문서
  * yarn+ version: [https://yarnpkg.com/features/workspaces](https://yarnpkg.com/features/workspaces)
  * CLI yarn 1.22 version: [https://classic.yarnpkg.com/en/docs/cli/](https://classic.yarnpkg.com/en/docs/cli/)
