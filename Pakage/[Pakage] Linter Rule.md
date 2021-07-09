# [Linter] Effective Dart

Property: Linter
TODO?: DONE
Tags: Level2

# ✔️ Linter란?

---

코드를 작성하기 위한 일종의 룰!

## ✨ 특징

[장점**]**

- 일관성을 통해서 다른 사람들이 코드를 배우고, 공유하기 쉽게 해준다.
- 흔하게 실수 할 수 있는 것들을 방지할 수 있다.

# ✔️ Linter 적용하기!

---

Depend on it

Run this Command:

with Flutter:

```dart
dev_dependcies
	effective_dart: ^1.3.2 (.Ver)    or 택 1

dependcies:
	linter: ^version
```

📌 `dependcies` 가 아닌 `dev_dependcies` 에 추가해주기

Make `analysis_options.yaml` :

```dart
include: package:effective_dart/analysis_options.yaml

linter:
  rules:
    lines_longer_than_80_chars: false
    public_member_api_docs: false
    avoid_positional_boolean_parameters: false
    non_constant_identifier_names: false

analyzer:
    errors:
      missing_required_param: error
      prefer_const_constructors: warning
```

✨ **참고**

1. false 면 Rule 무시
2. true 면 Rule 적용

3. Error 면 에러 
4. Warning 면 경고

### ✨ analyzer 란?

analyzer는 빌드 전 개발자가 실수할 수도 있는 소스코드를 분석해서 개발자에게 안내를 해주게 되는데 오류 소스를 다른 형태로 안내할 수 있도록 설정을 할 수 있습니다. 아무런 설정 하지 않고 linter에 룰만 추가한다면 default로 info로 설정이 되어 개발자에게 안내만 하게 됩니다.
(중요! 첫줄에 analyzer를 입력할 때 앞에 2칸을 띄워줘야 합니다.)

# ✔️ Reference

---

- [https://dart-lang.github.io/linter/lints/](https://dart-lang.github.io/linter/lints/) : Linter 룰 모음집
- [https://dev-yakuza.posstree.com/ko/flutter/linter/](https://dev-yakuza.posstree.com/ko/flutter/linter/)
- [https://sudarlife.tistory.com/entry/Flutter-플러터-Linting-설정으로-흔하게-실수할-수-있는-것을-build-전-방지하기](https://sudarlife.tistory.com/entry/Flutter-%ED%94%8C%EB%9F%AC%ED%84%B0-Linting-%EC%84%A4%EC%A0%95%EC%9C%BC%EB%A1%9C-%ED%9D%94%ED%95%98%EA%B2%8C-%EC%8B%A4%EC%88%98%ED%95%A0-%EC%88%98-%EC%9E%88%EB%8A%94-%EA%B2%83%EC%9D%84-build-%EC%A0%84-%EB%B0%A9%EC%A7%80%ED%95%98%EA%B8%B0) : Good Document