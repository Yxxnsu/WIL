# Dartdoc

## ✔️ Dartdoc  이란!?

주석만 달아주면 자동으로 dart 문서화를 해주는 dart 내 명령어 이자 Package 입니다.

### 1. 설정 및 규칙

문서화 하기전에 설명이 필요한 모든 함수나 변수에 `///` 를 이용하여 주석 처리를 해두어야 합니다. ☺️ 주석 처리에 대한 규칙은 아래의 공식 규칙 문서에서 보다 더 자세하게 확인하실 수 있습니다.

- 규칙: [https://dart.dev/guides/language/effective-dart/documentation](https://dart.dev/guides/language/effective-dart/documentation)
- 기본설정
    1. 프로젝트에 dartdoc dependency 추가!

        > dartdoc: ^1.0.2

    2. Terminal 에서 `vim ~/.zshrc` 입력 (권장)
        1. `.bash_profile` 에 입력하는 방법도 있다.
    3. export PATH="$PATH:/Users/im-yeonsu/Developer/flutter/bin/cache/dart-sdk/bin"  

        → "i" 누르면 `insert` 로 바뀌고, 위의 명령어 입력 후 `ESC`→ `:wq!`   (중요)

- 실행: 문서화하기 원하는 프로젝트의 `Root` 에서 아래 명령어 입력

    > flutter pub global run dartdoc

### 2. 실행 사진

![스크린샷 2021-08-20 오후 6.52.17.png](Dartdoc%20df085b2b2f684c1fbd0d9c3ee0d0fea0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.52.17.png)

🔥 Success! 된 것을 볼 수 있다. 

 

### 3. 문서 확인 : 프로젝트 파일 > doc > api > `index.html`

   → `html` 형태로 문서화가 됩니다. 

[참고]

![스크린샷 2021-08-20 오후 7.16.16.png](Dartdoc%20df085b2b2f684c1fbd0d9c3ee0d0fea0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-20_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.16.16.png)