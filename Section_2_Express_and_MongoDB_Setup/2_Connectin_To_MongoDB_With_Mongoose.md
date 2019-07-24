# Connecting To MongoDB With Mongoose

## STEP_1: config MongoDB

상위에 `config` 디렉터리를 생성한다

```bash
mkdir config
```

config 디렉터리에 `default.json`, `db.js` 파일을 생성한다

```json
// config/default.json

{
  "mongoURI": "mongodb+srv://<id>:<password>@cluster0-szqbh.mongodb.net/test?retryWrites=true&w=majority"
}
// <id>, <password>는 atlas 클라우드에서 생성한 데이터베이스의 관리 계정을 넣는다.
```

```js
// config/db.js

const mongoose = require("mongoose");
const config = require("config");
const db = config.get("mongoURI");

const connectDB = async () => {
  try {
    await mongoose.connect(db, { useNewUrlParser: true });

    console.log("MongoDB Connected...");
  } catch (err) {
    console.error(err.message);
    // Exit process with failure
    process.exit(1);
  }
};

module.exports = connectDB;
```

## STEP_2: Connect Database

server.js 파일을 열어 아래 코드를 추가로 입력한다

```js
const connectDB = require("./config/db");

// Connect Database
connectDB();
```

## STEP_3: Run serve and MongoDB 접속 확인

```bash
[nodemon] starting `node server.js`
Server started on port 5000
MongoDB Connected...
```

데이터베이스가 정상적으로 연결되어 접속되었다.
