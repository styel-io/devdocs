# hashtag 기능 구현

## 1. 글 작성시 #으로 시작되는 문자열 정규식 활용하여 분류

```js
const re = new RegExp(/(#[0-9a-zA-Z가-힝]+)/, "g");

var searchString = req.body.text;
var matchArray;
var resultString = "<div>";
var first = 0;
var last = 0;
var hashtagArray = [];

// 각각의 일치하는 부분 검색
while ((matchArray = re.exec(searchString)) != null) {
  last = matchArray.index;
  resultString += searchString.substring(first, last);
  matchArray[0] = matchArray[0].replace(" ", "");
  resultString +=
    `<a href='/t/${matchArray[0].replace("#", "")}'>` + matchArray[0] + "</a>";
  first = re.lastIndex;
  standByTag = matchArray[0].replace("#", "");
  hashtagArray.push(standByTag);
}

// 문자열 종료
resultString += searchString.substring(first, searchString.length);
resultString += "</div>";

const newPost = new Post({
  hashtags: hashtagArray,
  text: resultString,
  imageurl: req.body.imageurl,
  name: user.name,
  avatar: user.avatar,
  user: req.user.id
});

const post = await newPost.save();

res.json(post);
```

## 2. 해시태그 기반 검색기능 구현

문제)
포스트 스키마에 저장된 데이터베이스에서 검색 시 전체 글을 검색 해야되서 퍼포먼스 떨어짐.

해결)
태그 스키마를 따로 생성 후 태그 콜렉션에 게시글 ID 저장

검색기능 구현시 아래와 같이 요청 처리

```js
router.get("/:tag", async (req, res) => {
  try {
    const tag = req.params.tag;
    const tagPostList = await Tag.findOne({ tag });
    let posts = [];

    for (let i = 0; i < tagPostList.postId.length; i++) {
      posts.push(await Post.findById(tagPostList.postId[i]));
    }
    res.json(posts);
  } catch (err) {
    res.status(500).send("Server Error");
  }
});
```

## 2. 해시태그 스키마 생성 및 분류

tag 스키마 생성

```js
const mongoose = require("mongoose");

const TagSchema = new mongoose.Schema({
  tag: {
    type: String
  },
  postId: [
    {
      type: String
    }
  ]
});

module.exports = Tag = mongoose.model("tag", TagSchema);
```

글 작성시 입력값에서 해시태그를 분류하고 tag컬렉션에 저장

```js
for (let i = 0; i < hashtagArray.length; i++) {
  const tag = await Tag.findOne({ tag: hashtagArray[i] });

  if (tag === null) {
    const newTag = new Tag({
      tag: hashtagArray[i],
      postId: post_id
    });
    await newTag.save();
  } else {
    tag.postId.unshift(post_id);
    console.log(tag);
    await tag.save();
  }
}
```
