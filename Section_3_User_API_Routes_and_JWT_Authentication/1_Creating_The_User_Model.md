# Creating The User Model

## STEP_1: Add folder and file

![models](https://user-images.githubusercontent.com/14961047/61792867-aa5baa80-ae58-11e9-80fe-02059c677830.png)

## STEP_2: Write code in User.js

```js
// models/User.js

const mongoose = require("mongoose");

const UserSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true,
    unique: true
  },
  password: {
    type: String,
    required: true
  },
  avatar: {
    type: String
  },
  date: {
    type: Date,
    default: Date.now
  }
});

module.exports = User = mongoose.model("user", UserSchema);
```
