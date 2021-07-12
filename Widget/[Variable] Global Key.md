# [Variable] Global Key

Tag: GlobalKey
속성: 완료

# ✔️ Global Key

Global Key는 위젯을 참조 할 수 있는 키다.

## ✔️ Global Key 선언하기

```dart
final _formKey = GlobalKey<FormState>();
final scaffoldKey = GlobalKey<ScaffoldState>();

```

## ✔️ Key에 속성을 넣는다

```dart
Scaffold(
  key: scaffoldKey,
),

Form(
  key: _formKey,
),
```

## ✔️ Key를 사용

```dart
ButtonBar(
    children: <Widget>[
    ElevatedButton(
       onPressed: () {
          if(_formKey.currentState.validate()) {
            Navigator.pop(context);
           }
       },
       child: Text('SIGN UP'),
     ),
     ],
),
```