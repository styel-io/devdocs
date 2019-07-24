# Install Dependencies & Basic Express Setup

Express 서버를 동작시키기 위해 Node에 의존성 모듈을 설치하고 기본적인 설정을 하자  
VScode를 켜고 터미널을 띄우자

## STEP_1: .gitignore

`.gitignore` 파일을 생성하고 `node_modules/` 코드를 입력하고 저장한다  
node_modules 디렉터리에는 의존성 패키지모듈이 무수히 많이 설치되기 때문에 git으로 관리하지 않는다.

## STEP_2: Initialize

터미널 아래의 명령어를 입력하여 `package.json` 파일을 생성한다
`package.json`파일에는 의존성 패키지 모듈을 관리하고 프로젝트 전반적으로 필요한 내용을 담게 된다

```bash
yarn init
```

## STEP_3: Install Dependencies

```bash
yarn add express express-validator bcryptjs config gravatar jsonwebtoken mongoose request

yarn add -D nodemon concurrently
```

## STEP_4: Add Main entry file - server.js

`server.js` 파일을 생성하고 express server를 동작시키기 위해 아래 코드를 작성하여 초기 설정을 한다

```js
const express = require("express");

const app = express();

// Router
app.get("/", (req, res) => res.send("API Running"));

const PORT = process.env.PORT || 5000;

app.listen(PORT, () => console.log(`Server started on port ${PORT}`));
```

## STEP_5: scripts

`package.js` 파일에 `scripts`를 추가해준다.

```json
"scripts": {
    "start": "node server",
    "server": "nodemon server"
}
```

## STEP_6: RUN

터미널에서 명령하여 서버를 동작시킨다

```bash
yarn run server

# or

npm run server
```

scripts 작성대로 nodemon과 server.js가 실행된다

## STEP_7: 확인

브라우저에서 `localhost:5000` 주소로 접속해서 확인해보자
