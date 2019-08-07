# Invalid Host header

## 문제

![Invalid Host header](https://user-images.githubusercontent.com/14961047/62190244-dd97bf80-b3ab-11e9-8f36-32a8d92e2487.png)

aws에 MERN Stack 설치 후 접속했으나 Invalid Host header를 반환함

## 해결

리액트가 설치된 디렉터리 client/ 디렉터리에 .env.development 파일을 생성하고 아래 코드를 작성한다

```
HOST=styel.io
```

---

## 참조

<https://facebook.github.io/create-react-app/docs/proxying-api-requests-in-development>
