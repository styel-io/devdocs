# React & Concurrently Setup

## STEP_1: Create React

### 1. 설치

루트디렉터리에서 아래 명령을 실행한다.

```bash
# client 디렉터리에 리액트를 생성
npx create-react-app client
```

### 2. 실행

```bash
cd client

yarn start
```

![localhost:3000](https://user-images.githubusercontent.com/14961047/61991592-250d0b80-b08d-11e9-894f-0ba3d41d6109.png)

## STEP_2: Setup Concurrently

### 1. concurrently 설정

node 서버와 react를 동시에 구동하기 위해서 concurrently 설정을 해준다.
`package.json` 파일에서 `"scripts"`를 작성해주자.

```json
"scripts": {
    "start": "node server",
    "server": "nodemon server",
    "client": "cd client && yarn start",
    "dev": "concurrently \"yarn run server\" \"yarn run client\""
  }
```

### 2. 실행 및 확인

```bash
yarn run dev
```

![yarn run dev](https://user-images.githubusercontent.com/14961047/61991637-f5123800-b08d-11e9-965e-6665c992f44f.png)

## STEP_3: Install dependencies

client 디렉터리로 이동하여 의존성 패키지 모듈을 설치한다

```bash
cd client

yarn add axios react-router-dom redux react-redux redux-thunk redux-devtools-extension moment react-moment
```

| React Modules                                         |                                                                                    |
| ----------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Promise based HTTP client for the browser and node.js | [axios](https://www.npmjs.com/package/axios)                                       |
| DOM bindings for React Router                         | [react-router-dom](https://www.npmjs.com/package/react-router-dom)                 |
| predictable state container                           | [redux](https://www.npmjs.com/package/redux)                                       |
| React bindings for Redux                              | [react-redux](https://www.npmjs.com/package/react-redux)                           |
| Thunk middleware for Redux                            | [redux-thunk](https://www.npmjs.com/package/redux-thunk)                           |
| Redux DevTools Extension's helper                     | [redux-devtools-extension](https://www.npmjs.com/package/redux-devtools-extension) |
| date library                                          | [moment](https://www.npmjs.com/package/moment)                                     |
| React component for the moment date library           | [react-moment](https://www.npmjs.com/package/react-moment)                         |

## STEP_4: Delete unnecessary files

루트 디렉터리에 .gitignore 파일이 생성되어 있기 때문에 client 디렉터리의 client/.gitignore 파일은 삭제하자.

```bash
rm -rf .gitignore README.md .git
```

## STEP_5: Setup Proxy

proxy설정을 통해 client에서 전체경로를 입력하지 않아도 되게한다.
`client/package.json` 파일을 열어 추가해주자

```bash
"proxy": "http://localhost:5000"
```

package.json 전체 코드

```bash
{
  "name": "client",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "axios": "^0.19.0",
    "moment": "^2.24.0",
    "react": "^16.8.6",
    "react-dom": "^16.8.6",
    "react-moment": "^0.9.2",
    "react-redux": "^7.1.0",
    "react-router-dom": "^5.0.1",
    "react-scripts": "3.0.1",
    "redux": "^4.0.4",
    "redux-devtools-extension": "^2.13.8",
    "redux-thunk": "^2.3.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "proxy": "http://localhost:5000"
}
```
