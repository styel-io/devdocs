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
var standByTag = "";

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

const post_id = post.id;

for (let i = 0; i < hashtagArray.length; i++) {
  console.log(hashtagArray[i]);
  console.log(post_id);
  const tag = await Tag.findOne({ tag: hashtagArray[i] });

  console.log(tag);

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

## 2. 해시태그 기반 검색기능 구현

## 2. 해시태그 기반 검색기능 구현
