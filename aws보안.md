# 사건발생 - AWS 털리다!

## 문제 감지

일요일 저녁 아들과 데이트를 하고 있던 도중 아마존에서 메일이 몇 개 날아왔다.
내용은 아래와 같다.

![스크린샷 2019-09-01 오후 3 56 20](https://user-images.githubusercontent.com/14961047/64073085-1b9e4100-ccd4-11e9-8cce-1a8a3711cf15.png)

나의 친구 Sam이 대충 이렇게 얘기를 해서 보냈다.

"야 styel아, 너 좆된거 같은데?"

아니 이게 뭐지? 싶어서 aws 인스턴스를 보아하니 아래와 같은 상황이 벌어져 있었다.

![스크린샷+2019-08-27+오후+7 46 23](https://user-images.githubusercontent.com/14961047/64073125-89e30380-ccd4-11e9-8300-26f5de5d292d.png)
위 이미지는 해킹되어 생성된 인스턴스를 모두 종료하고 난 뒤다.

## 문제 해결

원인을 알기전에 해결부터 해야했다.

1. 해킹되어 실행되고 있는 인스턴스를 모두 종료시킨다.

2. 계정 비밀번호를 변경한다.

3. api로 aws에 접근 가능한 액서스 키를 삭제하고 새로 발급한다.

## 원인 분석

환경변수를 저장해놓은 .env 파일을 팀원에게 이메일로 보내는 과정에서 파일의 숨김 속성이 삭제되었다. 따라서 github에 env파일로 올려지게 되었다.

.env 파일에는 아래와 같이 aws 액서스키, 시크릿 액서스키가 들어가 있다.

```
aws_access_key_id=AKIAIGL5BL7BNA32AZHA

aws_secret_access_key=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
```

## 사고 사례

사례1.
![스크린샷 2019-09-01 오후 4 52 05](https://user-images.githubusercontent.com/14961047/64073444-eea05d00-ccd8-11e9-8317-5ad64e5b4c26.png)

사례2.

![스크린샷 2019-09-01 오후 5 00 44](https://user-images.githubusercontent.com/14961047/64073539-6c189d00-ccda-11e9-9dc9-21dc89f30cb7.png)

![스크린샷 2019-09-01 오후 5 07 45](https://user-images.githubusercontent.com/14961047/64073578-0a0c6780-ccdb-11e9-8f3b-90115493aa97.png)
