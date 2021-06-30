# [State] GetX

- [Hyper Link]

# ✔️ GetX란?

---

**GetX란 Flutter State Management 중 하나이며,
개발을 좀 더 쉽게 해주는 강력한 라이브러리 혹은 미니 프레임워크라고 볼 수 있다.**

### ✨ 특징

[**성능]**

- 성능과 자원의 소비를 최소화하는 것에 집중한다.
- StatefulWidget이나 Streams, ChangeNotifier를 사용하지 않는다.

**[생산성]**

- 적은 문법과 규칙으로 쉽고 간결하게 구현이 가능하다.
- 사용하지 않는 자원은 메모리에서 자동으로 제거해주므로 컨트롤러 제거를 신경쓰지 않아도 된다.

**[조직화]**

- route를 이동하거나 controller를 사용할 때, context가 필요없다.
- View, presentation logic, business logic, dependency injection, navigation 을 분리할 수 있다.
- presentation logic, business logic을 자체 DI기능을 통해 visualization layer에서 완벽하게 decoupling 시켜준다.
- MultiProvider를 사용하기 때문에, Controllers/Models/Bloc 클래스를 위젯 트리에 추가할 필요가 없다.

### ✨ 장점

1. GetX의 많은 기능으로 Route 관리, State 관리, 종속성 관리가 매우 편리하다.
    - ✨ 초보자도 사용 쌉가능!
2. 상태 관리 등을 통해 로직 분리가 가능해서 코드 구조를 간결하게 짜기 쉽다.
3. 시스템적인 측면에서 최적화가 잘되어 있다.

# ✔️ GetX 적용하기

---

Depend on it

Run this Command:

with Flutter:

```dart
dependcies
	get: ^version (ex.^4.0.1)
```

This will add a line like this to your package pubspec.yaml (run an implicit `dart pub get`) 

import:

```dart
import 'package:get/get.dart';
```

## 🙌 주요 기능

---

### 가. 라우트(Route) 관리

- **1) MaterialApp -> GetMaterialApp**

    ```
    void main() {
      runApp(GetMaterialApp(home: Home()); // Before: runApp(MaterialApp(
    }

    ```

- **2) Navigation**
    - **[Get.to](http://get.to/)**
    새로운 화면으로 이동 할 때 :

    ```
    //기존
    Navigator.of(context).push(
      MaterialPageRoute(
     	 builder: (BuildContext context) => HomePage(),
      ),
    );

    // GetX
    Get.to(HomePage());

    ```

    - **Get.toNamed**
    name으로 새로운 화면으로 이동 할 때 :

    ```
    // 기존
    Navigator.pushNamed(context, '/home');

    // GetX
    Get.toNamed('/home');

    // 사용하기 전에 name 설정하기(main.dart)
    getPages: [
    	GetPage(name: '/home', page: () => Home()),
    ]

    ```

    - **Get.back**
    화면 나가기 (닫기)

    ```
    //기존
    if(Navigator.of(context).canPop()){
    Navigator.pop(context);
    }

    // GetX 방식
    Get.back();

    ```

    ---

    - **Get.off**
    화면 이동 후, 이전 화면 닫기

    ```
    // 기존
    Navigator.of(context).pushReplacement(
    	MaterialPageRoute(
    		builder: (BuildContext context) => NextScreen(),
    	),
    );

    // GetX
    Get.off(NextScreen());

    ```

    - **Get.offAll**
    화면 이동 후, 모든 페이지 스택 삭제하기

    ```
    // 기존
    navigator.pushAndRemoveUntil(
        MaterialPageRoute(builder: (BuildContext context) => NextScreen()),
        ModalRoute.withName('/next'),
      );

    // GetX
    Get.offAll(NextScreen());

    ```

- **3) Argument 관리**
    - return되는 값 받기

    ```
    // [home] 받아오는 입장 (next -> home)
    Text(
    	'리턴값 : $returnValue',
    ),
    RaisedButton(
    	onPressed: () async {
    		final reVal = await Get.to(Next());

    		setState(() {
    			returnValue = reVal;
    		});
    	},
    )

    // [next] 넘겨주는 입장 (next -> home)
    RaisedButton(
    	onPressed: (){
    		Get.back(result: resultVal);
    	},
    )

    ```

    - argument 값 넘겨주기

    ```
    // [home] argument 보내는 입장 (home -> next)
    Get.to(Next(), arguments: '안녕!');

    //[next] argument 받는 입장
    Text(Get.argument),

    ```

- **4) Transition 사용하기**

    다양한 transition을 사용하여 훨씬 dynamic한 page 전환이 가능하다.

    ```
    Get.to(
    	next(),
    	transition: Transition.fadeIn,
    )

    ```

- **5) Named Route**

    Named Route를 사용하여 변수나 query를 주고 받을 수 있다.

    ```
    //미리 named route 설정
    getPages: [
    	GetPage(
    		name: '/home',
    		page: () => Home(),
    	),
    	GetPage(
    		name: '/next/:param',
    		page: () => Next(),
    	),
    ]

    // [home] 파라미터 보내주기
    Get.toNamed('/next/hello?id=1234&name=yj')

    // [next] 파라미터 받아서 사용하기
    Get.parameters['param']  //hello
    Get.parameters['id']  //1234
    Get.parameters['name']  //yj

    ```

### 나. 상태 관리(State Management)

- **1) Update Statement**

    ### Update 상태 관리

    On Update를 통한 상태관리 사용하기!

    ✨ DES: Reactive 상태관리 비해 메모리를 적게 사용하기 때문에 일반적인 경우에 사용!

    **1) On Update Controller 생성하기** 

    ```dart
     // Controller 생성
     class Controller extends GetxController{
      int count = 0
      increment(){
      	count++;
        update();  // **📌 필수!**
      }
    }
    ```

    **2) On Update Controller 사용하기** 

    - Update를 통한 상태관리를 이용할 때는 `GetBuilder` 를 **꼭**!! 사용해야한다.

    ```dart

    # final controller = Get.put(Controller()); => 바깥에 선언해주면 init 필요(x)

    GetBuilder<Controller>(
    	init: Controller(), // ✅ 바깥에서 선언하지 않았을 때 사용한다.
    	builder: (_) {
        	return Text('count : ${_.count}');
        },
    ),

    ## Using Controller

    🌼 Case: **내부** init

    RaisedButton(onPressed: () {
        Get.find<Controller>().increment();
      },
    ),

    🌼 Case: **바깥** init

    RaisedButton(onPressed: () {
        controller.increment();
      },
    ),

    P.S 뭘 사용할지는 Your Choice ><
    ```

    **3) Get.find VS Get.put** 

    - **Get.find**: 다른 곳에서 이미 사용하고 있는 GetController를 현재 클래스에서 사용할 수 있도록 한다.
    - **Get.put**: 모든 Child Route에서 사용 가능할 수 있도록 한다.

    ```dart
    class Home extends StatelessWidget {

      @override
      Widget build(context) {

        // Instantiate your class using Get.put() to make it available for all "child" routes there.
        final Controller c = Get.put(Controller());

        return Scaffold{
    			body: FloatingActionButton(
    				child: Icon(Icons.add),
    				onPressed: c.increment()),  ()=> Using Controller
    			);
    		}

    class Other extends StatelessWidget {
      // You can ask Get to find a Controller that is being used by another page and redirect you to it.
      final Controller c = Get.find();

      @override
      Widget build(context){
         // Access the updated count variable
         return Scaffold(body: Center(child: Text("${c.count}")));
      }
    }
    ```

- **2) Reactive Statement**

    ### Reactive 상태관리

    Reactive를 통한 상태관리 사용하기!

    ✨ DES: `Update` 방식에 없는 특별한 기능이 있는데 그것은 바로 `Worker` 이다. Workers를 사용하면 다양한 상황 별로 적절한 대응을 하도록 구현할 수 있다.

    **1) Reactive Controller 생성하기** 

    - Reactive를 통한 상태관리에서는 변수 타입을 지정할 때 `Rx~` 또는 `var` 타입으로 지정 해준다.
    - 또한 값을 대입할 때 `.obs` 를 붙여 준다.
    - ❗️ [중요] Reactive 방식에서는 `Update()` 필요없다!

    ```dart
    class ReactiveController extends GetxController{

    	// 변수 타입
    	RxInt count = 0.obs; //1번    또는
      var count = 0.obs;  //2번

    	// 변수 초기화
    	var count = 0.obs; // 1번           또는
      var count = Rx<int>(0); // 2번      또는
      var count = RxInt(0); // 3번

    	// Func () => Update 필요없다.
    	void increment() => count.value++;

    	//위에서 "value" 를 붙이는 것은 타입에 따라 다르다.

    			1. Primitive type => .value 붙여야 한다
    				- int, String etc..

    			2. Non - Primitive => .value 안붙여도 된다.
    				- List

    }

    P.S Users Choice :)

    ```

    **2) Reactive Controller 사용하기** 

    - Simple 방식의 `GetBulider` 와 같은 역할을 하는 것이 GetX이다.
    - `GetX` 보다 더 간단한 방법은 `Obx()` 를 사용하는 것아다. `Obx()` 의 경우 사용할 컨트롤러 종류 따로 명시 필요 없고 보여줄 `Widget` 만 리턴해주면 된다.
    - `Obx()` 를 사용할 때는 반드시 `Get.put()`을 필요로 한다!

    ```dart
    # 1. Builder
    - GetX() 
    ————————————————————————————————————
    [Code]

    GetX<ReactiveController>(
    	init: ReactiveController(), // GetX안 init

    			builder: (_) {

        	return Text('count 1 : ${_.count1.value}');
    			//위에서 value를 붙이는 것은 타입에 따라 다르다.

    			1. Primitive type => .value 붙여야 한다
    				- int, String etc..

    			2. Non - Primitive => .value 안붙여도 된다.
    				- List
        },
    ),

    # 2. Builder
    -  Obx()
    ————————————————————————————————————
    [Code]

    //📌 무조건 해줘야 한다!
    final controller = Get.put(controller());

    Obx(() {
      return Text(
        'clicks: ${controller.count2.value}',
      );
    }),

    ————————————————————————————————————

    ## Using Controller

    🌼 Case: **내부** init => Builer init:

    RaisedButton(onPressed: () {
        Get.find<ReactiveController>().increment();
      },
    ),

    🌼 Case: **바깥** init => Get.put()

    RaisedButton(onPressed: () {
        controller.increment();
      },
    ),

    P.S 뭘 사용할지는 Your Choice ><

    ```

    **3) Workers 사용하기** 

    > Workers 를 사용하면 Rx들의 변화를 감지하고 다양한 상황 별로 적절한 대응을 하도록 구현할 수 있다.

    [종류]

    ```dart
    1. count2가 처음으로 변경되었을 때만 호출된다.
    once(count2, (_) {
      print('$_이 처음으로 변경되었습니다.');
    });

    2. count2가 변경될 때마다 호출된다.
    ever(count2, (_) {
      print('$_이 변경되었습니다.');
    });

    3. count2가 변경되다가 마지막 변경 후, 1초간 변경이 없을 때 호출된다.
    debounce(count2,(_) {
      print('$_가 마지막으로 변경된 이후, 1초간 변경이 없습니다.');
    },
      time: Duration(seconds: 1),
    );

    4. count2가 변경되고 있는 동안, 1초마다 호출된다.
    interval(
      count2,
      (_) {
        print('$_가 변경되는 중입니다.(1초마다 호출)');
      },
      time: Duration(seconds: 1),
    );

    ```

    [사용]

    - Controller Class에 `onInit()` Override 하고, `super.onInit()` 을 먼저 호출한다.
    - 그 다음 사용하고자 하는 `Worker` 를 등록해주면 된

    ```dart
    void onInit() {
      super.onInit();

      once(count2, (_) {
        print('$_이 처음으로 변경되었습니다.');
      });

      ever(count2, (_) {
        print('$_이 변경되었습니다.');
      });

      debounce(count2, (_) {
        print('$_가 마지막으로 변경된 이후, 1초간 변경이 없습니다.');
      },
       time: Duration(seconds: 1),
    	);

      interval(count2,(_) {
        print('$_가 변경되는 중입니다.(1초마다 호출)');
      },
        time: Duration(seconds: 1),
      );
    }
    ```

    ### 4) 전체 코드

    - Controller.dart

        ```dart
        import 'package:get/get.dart';

        class Controller extends GetxController {
          var count1 = 0;
          var count2 = 0.obs;
          // var count2 = Rx<int>(0);
          // var count2 = RxInt(0);

          void increment1() {
            count1++;
            update();
          }

          void increment2() => count2.value++;

          @override
          void onInit() {
            super.onInit();

            once(count2, (_) {
              print('$_이 처음으로 변경되었습니다.');
            });
            ever(count2, (_) {
              print('$_이 변경되었습니다.');
            });
            debounce(
              count2,
              (_) {
                print('$_가 마지막으로 변경된 이후, 1초간 변경이 없습니다.');
              },
              time: Duration(seconds: 1),
            );
            interval(
              count2,
              (_) {
                print('$_가 변경되는 중입니다.(1초마다 호출)');
              },
              time: Duration(seconds: 1),
            );
          }
        }
        ```

    - Main.dart

        ```dart
        import 'package:flutter/material.dart';
        import 'package:get/get.dart';

        import 'controller.dart';

        void main() {
          runApp(App());
        }

        class App extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            return MaterialApp(
              title: 'Getx example',
              home: HomePage(),
            );
          }
        }

        class HomePage extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            final controller = Get.put(Controller());
            return Scaffold(
              appBar: AppBar(
                title: Text('Getx example'),
              ),
              body: Center(
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    GetBuilder<Controller>(
                      init: Controller(),
                      builder: (_) => Text(
                        'clicks: ${_.count1}',
                      ),
                    ),
                    TextButton(onPressed: controller.increment1, child: Text('increment1')),
                    GetX<Controller>(
                      builder: (_) => Text(
                        'clicks: ${_.count2.value}',
                      ),
                    ),
                    Obx(() {
                      return Text(
                        'clicks: ${controller.count2.value}',
                      );
                    }),
                    TextButton(onPressed: controller.increment2, child: Text('increment2')),
                  ],
                ),
              ),
            );
          }
        }
        ```

### 다. etc

- **1) snackbar**

    ```
    Get.snackbar(
    	'제목',
    	'내용',
    	snackPosition : SnackPosition.BOTTOM,
    );

    ```

- **2) dialog**

    ```
    //getx만 사용하여 매우 간단하게 구현
    Get.defaultDialog(middleText: 'Dialog');

    //실제 dialog widget을 사용하여 구현도 가능
    Get.dialog(widget); //widget은 실제 dialog

    ```

- **3) bottomSheet**

    ```
    Get.bottomSheet(
    	Container(
    	color: Colors.whire,
    	child: Wrap(
    		children: <Widget>[
    			ListTile(
    				leading: Icon(Icons.music_note),
    				title: Text('Setting'),
    				onTap: () => {}),
    			],
    		),
    	),

    ```

# ✔️ 출처

---

1. [https://terry1213.github.io/flutter/flutter-getx/](https://terry1213.github.io/flutter/flutter-getx/)
2. [https://dunchi.tistory.com/102](https://dunchi.tistory.com/102)
3. [https://velog.io/@chjo0330/Flutter-GetX를-통한-상태관리](https://velog.io/@chjo0330/Flutter-GetX%EB%A5%BC-%ED%86%B5%ED%95%9C-%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC)
4. [https://www.youtube.com/watch?v=wgJItCEL7hk&t=368s](https://www.youtube.com/watch?v=wgJItCEL7hk&t=368s)
