# Request & Body Validation

클라이언트에서 데이터를 전송하기 위해 서버에 값을 전달하면 서버는 그 값을 제대로 받아서 처리해야한다.
express에서 request body 값을 전달받을 수 있다

## STEP_1: 미들웨어 설정

클라이언트에서 보낸 데이터를 항상 받아서 처리 할 수 있도록 미들웨어 설정을 해준다
`server.js`에 아래 내용을 추가하자

```js
// server.js

// Init Middleware
app.use(express.json({ extended: false }));
```

`server.js` 전체 코드는 다음과 같다

```js
// server.js

const express = require("express");
const connectDB = require("./config/db");

const app = express();

// Connect Database
connectDB();

// Init Middleware
app.use(express.json({ extended: false }));

app.get("/", (req, res) => res.send("API Running"));

// Define Routes
app.use("/api/users", require("./routes/api/users"));
app.use("/api/auth", require("./routes/api/auth"));
app.use("/api/profile", require("./routes/api/profile"));
app.use("/api/posts", require("./routes/api/posts"));

const PORT = process.env.PORT || 5000;

app.listen(PORT, () => console.log(`Server started on port ${PORT}`));
```

## STEP_2: 요청된 값 확인하기

`localhost:5000/api/users`에 post 방식으로 데이터를 넘겨줘서 정상적으로 처리되는지 확인해보자

`routes/api/users.js` 파일을 아래 내용으로 바꾸자

```js
const express = require("express");
const router = express.Router();

// @route    POST api/users
// @desc     Register user
// @access   Public
router.post("/", (req, res) => {
  console.log(req.body);
  res.send("User route");
});

module.exports = router;
```

console에서 받은 데이터 값을 출력하도록 했다

postman을 이용하여 값을 전달해보자
![api/users](https://user-images.githubusercontent.com/14961047/61794083-6a49f700-ae5b-11e9-84e1-88a6157b86ce.png)

![req.body](https://user-images.githubusercontent.com/14961047/61794229-b301b000-ae5b-11e9-842c-7d5d6a03cc78.png)

정상적으로 값을 받아오는 것을 콘솔에서 확인 할 수 있다

## STEP_3: Validate request

요청값에 대한 규칙을 설정하고 응답한다
`routes/api/users.js` 파일을 아래와 같이 변경하자

```js
// routes/api/users.js

const express = require("express");
const router = express.Router();
const { check, validationResult } = require("express-validator"); // https://express-validator.github.io/docs/ 사용법

// @route    POST api/users
// @desc     Register user
// @access   Public
router.post(
  "/",
  [
    check("name", "Name is required")
      .not()
      .isEmpty(),
    // username must be an email
    check("email", "Please include a valid email").isEmail(),
    // password must be at least 6 chars long
    check(
      "password",
      "Please enter a password with 6 or more characters"
    ).isLength({ min: 6 })
  ],
  (req, res) => {
    const errors = validationResult(req);
    // Finds the validation errors in this request and wraps them in an object with handy functions
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    res.send("User route");
  }
);

module.exports = router;
```

postman을 이용하여 검증해보자

![/routes/api/users](https://user-images.githubusercontent.com/14961047/61806043-f2d39200-ae71-11e9-907e-195dd54357cf.png)
name 값만 전달했더니 email과 password가 정확한 값이 들어오지 않아 에러를 나타낸다  
Status는 400 Bad Request를 나타낸다
<br><br><br>
![/routes/api/users 200 Ok](https://user-images.githubusercontent.com/14961047/61837300-1ffa6180-aebf-11e9-8c02-a8388e586d91.png)

요청 값을 모두 충족시켰더니 정상적으로 값을 반환한다

![saved database](https://user-images.githubusercontent.com/14961047/61837403-88e1d980-aebf-11e9-9116-81c3d43650d6.png)
데이터베이스에 패스워드 값이 해쉬화 되어 정상적으로 저장이 되었다.
