# Pub.dev

## ✔️ Pub.dev에 자신의 package를 업로드해보자!

Pub.dev에 자신의 package를 명령어 두개로 쉽게 pub.dev에 업로드할 수 있어요!

### [1] Package 구성요소

- package의 최소 구성요소로는
    - pubspec.yaml 파일 ( package name, version, author 등이 명시되어 있어야 한다)
    - lib 디렉토리 ( 코드가 있는 )

### [2] [`pub.dev`](http://pub.dev) 에 올라가기 위해 필요한 것 (중요)

- 소스 코드가 들어있는 Github의 주소
- License 파일 → Repository 만들 때 생성
    - 이미지

        ![스크린샷 2021-08-21 오후 1.31.46.png](Pub%20dev%201d7399b27f1a4ca49206a50b203dc84c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-21_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.31.46.png)

- pubspec.yaml
    1. name 
    2. description 
    3. version : 1.0.0 (권장) 
    4. homepage: github 주소 적어주세요.
- [ChangeLog.md](http://changelog.md)
- [Readme.md](http://readme.md)
- main.dart : 본인의 package or plugin을 사용하기 위해 참고할 `example` 코드.

📌 `Pub.dev` 에 올라가기 위해 총 6가지가 필요합니다!  

### [3] pub.dev에 upload 하기

- 우선 아래 명령어로 `pub.dev` 에 올라갈 수 있는 조건을 충족 했는지 확인 해줍니다.

```dart
$ flutter pub publish --dry-run
```

- 충족이 됐고, 성공이 됐다면 아래 명령어를 입력합니다.

```dart
$ flutter pub
```

그러면 pub.dev에 올라가 있는 본인의 package를 확인할 수 있습니다.

![스크린샷 2021-08-21 오후 1.40.51.png](Pub%20dev%201d7399b27f1a4ca49206a50b203dc84c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-21_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_1.40.51.png)

📌 유의사항: 한번 올리면 내릴 수 없으므로 신중하게 올릴 것! (중요)

- 만약 실수로 올렸을 땐 반드시 `48시간` 안에 support@pub.dev 로 경위를 메일로 보내면 내려준다.

    (P.S 본인은 당연히 내릴 수 있을줄 알고 테스트를 다 하지 않은 상태에서 그냥 올려봤는데 내리는 것이 안되서 메일을 보내서 내렸다...)

### [4] [pub.dev](http://pub.dev) 정책

Link : [https://pub.dev/policy](https://pub.dev/policy)

 

🔗 시연링크: [https://youtu.be/JQ44_5erO6Q](https://youtu.be/JQ44_5erO6Q)