# Route Files With Express Router

## STEP_1: Add folder and files

![route](https://user-images.githubusercontent.com/14961047/61790978-96fa1080-ae53-11e9-9fff-659571260301.png)

```js
// routes/api/auth.js

const express = require("express");
const router = express.Router();

// @route    GET api/auth
// @desc     Test route
// @access   Public
router.get("/", (req, res) => res.send("Auth route"));

module.exports = router;
```

```js
// routes/api/posts

const express = require("express");
const router = express.Router();

// @route    GET api/posts
// @desc     Test route
// @access   Public
router.get("/", (req, res) => res.send("Posts route"));

module.exports = router;
```

```js
// routes/api/profile

const express = require("express");
const router = express.Router();

// @route    GET api/profile
// @desc     Test route
// @access   Public
router.get("/", (req, res) => res.send("Profile route"));

module.exports = router;
```

```js
// routes/api/users.js

const express = require("express");
const router = express.Router();

// @route    GET api/users
// @desc     Test route
// @access   Public
router.get("/", (req, res) => res.send("User route"));

module.exports = router;
```

## STEP_2: 라우터 생성

미들웨어를 이용하여 routes/api/ 디렉터리에 작성한 파일로 접근하여 처리되도록 한다
server.js에 아래 코드를 추가한다

```js
// server.js

// Define Routes
app.use("/api/users", require("./routes/api/users"));
app.use("/api/auth", require("./routes/api/auth"));
app.use("/api/profile", require("./routes/api/profile"));
app.use("/api/posts", require("./routes/api/posts"));
```

server.js의 전체 코드는 다음과 같다

```js
// server.js

const express = require("express");
const connectDB = require("./config/db");

const app = express();

// Connect Database
connectDB();

app.get("/", (req, res) => res.send("API Running"));

// Define Routes
app.use("/api/users", require("./routes/api/users"));
app.use("/api/auth", require("./routes/api/auth"));
app.use("/api/profile", require("./routes/api/profile"));
app.use("/api/posts", require("./routes/api/posts"));

const PORT = process.env.PORT || 5000;

app.listen(PORT, () => console.log(`Server started on port ${PORT}`));
```
