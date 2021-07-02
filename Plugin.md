# Plugin

- [Hyper Link]

# ✔️ Plugin 이란?

---

우리는 여러 플랫폼에서 어떤 프로그램을 개발할 때 수 많은 라이브러리들을 가져다가 쓴다.

### ✨ 언어

- Android : Java / Kotlin 사용
- iOS : Objective C / Swift 사용

### ✨ 장점

- Plugin을 만들어 두면 개발할 때 다시 만들지 않아 효율적이고 모듈화를 통해 코드간의 Dependency를 줄여주며 안정적인 소프트웨어를 만들 수 있다.
- 다른 사람들이 만들어둔 Plugin과 내가 만든 Plugin을 조합해서 또 다른 소프트웨어를 만들 수 있다.

### ✨ Plugin VS Package

---

📌 Flutter plugins

[Defintion]

- 플러그인은 Android/iOS의 기본 기능 사이에서 발생하는 일종의 다리

[Description]

- Flutter plugin은 android 및 ios와 같은 네이티브 코드의 Wrappe이다.
- Flutter는 플랫폼 채널과 메세지 전달을 통해 네이티브 App이 할 수 있는 모든 것을 할 수 있다.

📌  Flutter package 

[Defintion]

- 패키지는 Dart만을 사용하여 작성된 어플리케이션

[Description]

- Util 라이브러리의 코드를 사용하여 개발 속도를 높여주는 도구(?)다.

# ✔️ Plugin 만들어보기 - (Background Color)

## 🙌  Plugin Setting

### 1) Command Line 사용

```dart
$ flutter create --template=plugin --org com.example --template=plugin --platforms=android,ios -a java(사용언어) -i objc(사용언어) plugin_codelab
$ cd plugin_codelab
$ dart migrate --apply-changes
$ cd example
$ flutter pub upgrade
$ dart migrate --apply-changes
```

### 2) AndroidStudio 사용

- ✨ New Flutter Project에서 Flutter Plugin으로 생성

![Plugin%206c4034ce2ba34289b0830e245eb2ad68/Untitled.png](Plugin%206c4034ce2ba34289b0830e245eb2ad68/Untitled.png)

## 🙌 Plugin Code 작성

---

- **Plugin 기본 구조**

![Plugin%206c4034ce2ba34289b0830e245eb2ad68/Untitled%201.png](Plugin%206c4034ce2ba34289b0830e245eb2ad68/Untitled%201.png)

- **가. Plugin class**
    - **lib/flutter_hem_plugin.dart**

        flutter에 android/ios 함수들을 연결해주는 class

    ```dart
    import 'dart:async';
    import 'package:flutter/cupertino.dart';
    import 'package:flutter/services.dart';

    class FlutterHemPlugin {
      static const MethodChannel _channel =
          const MethodChannel('flutter_hem_plugin');

      static Future<Color> generateColor() async {
        final randomColor = await _channel.invokeListMethod('generateColor');

        return Color.fromRGBO(randomColor[0], randomColor[1], randomColor[2], 1.0);
      }
    }
    ```

    **1) MethodChannel (channel)**

    - platform(android/ios) native code를 dart에서 사용할 수 있도록 제공된 API

    **2) invokeListMethod('호출하려는 함수이름')**

    - 호출하려는 함수를 ios/android에서 처리하고 그 값을 받아온다.
    - 호출하려는 함수의 이름을 동일하게 입력하여야 한다.

- **나. Android (.kt)**
    - android/src/main/kotlin/com.example.flutter_hem_plugin/FlutterHemPlugin

    ```kotlin
    package com.example.flutter_hem_plugin

    import androidx.annotation.NonNull

    import io.flutter.embedding.engine.plugins.FlutterPlugin
    import io.flutter.plugin.common.MethodCall
    import io.flutter.plugin.common.MethodChannel
    import io.flutter.plugin.common.MethodChannel.MethodCallHandler

    /** FlutterHemPlugin */
    class FlutterHemPlugin: FlutterPlugin, MethodCallHandler {
      private lateinit var channel : MethodChannel  //channel 변수 생성

      override fun onAttachedToEngine(@NonNull flutterPluginBinding: FlutterPlugin.FlutterPluginBinding) {
    		channel = MethodChannel(flutterPluginBinding.binaryMessenger, "flutter_hem_plugin") 
        channel.setMethodCallHandler(this)
      }

      override fun onMethodCall(@NonNull call: MethodCall, @NonNull result: Result) {
        if (call.method == "generateColor") {
          val randomColor = generateColor()
          result.success(randomColor)
        }
        else {
          result.notImplemented()
        }
      }

      private fun generateColor(): List<Int>{
        return  arrayOf(0, 0, 0).map { (Math.random() * 256).toInt() }
      }
    }
    ```

    **1) onAttachedToEngine**

    - Flutter에서 넘겨준 함수를 변수에 저장한다.
    - **setMethodCallHandler 
    :** Flutter에서 변수에 저장한 함수를 받아올 MethodCallHandler를 channel에 등록한다.

    **2) onMethodCall**

    - 받아온 함수를 처리해서 리턴해준다.

- **다. IOS (.swift)**

    **1) register**

    - 내부적으로 먼저 실행되는 method로 해당 Plugin class를 Flutter와 연결한다.
    - registrar : plugin을 게시하는데 사용된 레지스터
    - FlutterMethodChannel : channel 생성

    **2) addMethodCallDelegate**

    - channel에서 call되어 오는 message를 받기 위해 현재 Plugin을 등록하는 메소드

    **3) handle**

    - call.method를 사용하여 실행할 함수를 넘겨준다.

- **라. flutter 실행 코드**
    - main.dart

    ```dart
    import 'package:flutter/material.dart';
    import 'package:flutter_hem_plugin/flutter_hem_plugin.dart';

    void main() {
      runApp(MyApp());
    }

    class MyApp extends StatefulWidget {
      @override
      _MyAppState createState() => _MyAppState();
    }

    class _MyAppState extends State<MyApp> {
      Color _bgColor;

      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          home: Scaffold(
            backgroundColor: _bgColor,
            appBar: AppBar(
              title: const Text('Plugin example app'),
            ),
            floatingActionButton: FloatingActionButton(
              onPressed: _handleGernerateColorPressed,
              child: Icon(
                Icons.color_lens,
              ),
            ),
          ),
        );
      }

      void _handleGernerateColorPressed() async{
        final randomColor = await FlutterHemPlugin.generateColor();

        setState(() {
          _bgColor = randomColor;
        });
      }
    }
    ```

    **1) FlutterHemPlugin**

    - FlutterHemPlugin 클래스 내에 함수를 호출해서 ios / android를 거쳐서 값을 받아온다.
    - 이로써 Flutter에서도 네이티브 App이 할 수 있는 모든 것을 할 수 있다.

# ✔️ 출처

---

1. MethodChannel class api: [https://api.flutter.dev/flutter/services/MethodChannel-class.html](https://api.flutter.dev/flutter/services/MethodChannel-class.html)
2. [https://flutter-ko.dev/docs/development/packages-and-plugins/developing-packages#api-documentation](https://flutter-ko.dev/docs/development/packages-and-plugins/developing-packages#api-documentation)
3. [https://codelabs.developers.google.com/codelabs/write-flutter-plugin?hl=ko#2](https://codelabs.developers.google.com/codelabs/write-flutter-plugin?hl=ko#2)
4. [https://wonjerry.github.io/2019/11/10/make-flutter-plugin-part1/](https://wonjerry.github.io/2019/11/10/make-flutter-plugin-part1/)
5. [https://stackoverflow.com/questions/54731733](https://stackoverflow.com/questions/54731733) 
6. [https://here4you.tistory.com/260](https://here4you.tistory.com/260)