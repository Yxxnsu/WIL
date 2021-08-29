# [Plugin] IOS widget 만들기

✔️ swift 언어를 사용하여 widget을 만들 수 있다.

✔️ naver weather widget을 구현하기 위해 scraper를 사용할 수 있다.

✔️ plugin을 사용하여 IOS widget과 flutter를 연결해볼 수 있다.

# 🔹 IOS widget

### **1) iOS widget이란?**

iOS widget은 ios의 home screen에 추가 할 수 있는 widget을 말한다.

### 2) widget setting 하기

- xcode에서 swift언어로 target으로 widget extension을 사용하여 만들 수 있다.
- xcode → file → new → target → widget extension

![%5BPlugin%5D%20IOS%20widget%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5%20ff8040eb979f4f3fa6265c96be0b0b07/Untitled.png](%5BPlugin%5D%20IOS%20widget%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5%20ff8040eb979f4f3fa6265c96be0b0b07/Untitled.png)

### 3. swift file method 분석

**1) GroupId**

- ios에서 widget을 생성하기 위해서는 GroupId를 등록하여야 한다.
- 등록된 GroupId는 method에서 사용된다.

```dart
private let widgetGroupId = "group.com.swfact.home"
```

**2) Provider Struct**

```dart
struct Provider: TimelineProvider {
    func placeholder(in context: Context) -> ExampleEntry {
        ExampleEntry(date: Date(), temperature: "Placeholder temperature", description: "Placeholder description", rainFall: "Placeholder rainFall", location: "Placeholder location")
    }
    
    func getSnapshot(in context: Context, completion: @escaping (ExampleEntry) -> ()) {
        let data = UserDefaults.init(suiteName:widgetGroupId)
        let entry = ExampleEntry(date: Date(), temperature: data?.string(forKey: "temperature") ?? "No temperature Set", description: data?.string(forKey: "description") ?? "No description Set", rainFall: data?.string(forKey: "rainFall") ?? "No rainFall Set", location: data?.string(forKey: "location") ?? "No location Set")
        completion(entry)
    }
    
    func getTimeline(in context: Context, completion: @escaping (Timeline<Entry>) -> ()) {
        getSnapshot(in: context) { (entry) in
            let timeline = Timeline(entries: [entry], policy: .atEnd)
            completion(timeline)
        }
    }
}
```

- **placeholder**
widget을 처음 정의할 때, 일반적인 ExampleEntry를 정의해 주는 함수이다.
- **getSnapshot**
사용자가 home에서 처음 widget을 등록할 때, widget gallery에 표현되는 형태를 정의해준다.
- **getTimeline**
초기에 snapshot을 요청한 이후에, 정기적으로 업데이트를 할 수 있는 timeline을 설정하는 함수이다.

**3) EntryView struct**

- widget의 UI를 설정하고 custom할 수 있는 struct이다.
- Swift UI를 사용한다. ⇒ View (body)

# 🔹 Flutter에서 naver 날씨 Scrap하기

### 1. package

```dart
import 'package:web_scraper/web_scraper.dart';
```

- web_scraper: ^0.1.4
- web에 있는 정보를 scrap해서 flutter에서 사용할 수 있게 해주는 package

### 2. 사용 방법

```dart
String siteUrl = 'https://weather.naver.com/';
--------------------------------------------------------------------------------

void getData() async {
    final webScraper = WebScraper(siteUrl);
    if (await webScraper.loadWebPage("")) {
      var _temperature =
      webScraper.getElement('div.weather_area > strong.current', ['title']);
      var _description = webScraper.getElement(
          'div.weather_area > p.summary > span.weather.before_slash',
          ['innerHtml']);
      var _rainFall = webScraper.getElement(
          'div.weather_area > dl.summary_list > dd.desc', ['innerHtml']);
      var _location = webScraper.getElement(
          'div.location_area > strong.location_name', ['innerHtml']);

      setState(() {
        temperature = _temperature[0]['title'].replaceAll(RegExp(r'현재 온도'), '');
        description = _description[0]['title'];
        rainFall = _rainFall[0]['title'];
        location = _location[0]['title'];
        loaded = true;
      });
    }
  }
```

- webScraper를 사용하여 siteUrl 주소의 웹을 scrap한다.
- getElement를 사용하여 html의 data를 가져올 수 있다.
- 해당하는 data들이 모두 list형태로 저장되므로 원하는 index에 접근하여 사용할 수 있다.

    ex) var _location = webScraper.getElement(
              'div.location_area > strong.location_name', ['innerHtml']); **⇒⇒⇒ "남구 상대동"**

    ![%5BPlugin%5D%20IOS%20widget%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5%20ff8040eb979f4f3fa6265c96be0b0b07/Untitled%201.png](%5BPlugin%5D%20IOS%20widget%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5%20ff8040eb979f4f3fa6265c96be0b0b07/Untitled%201.png)

# 🔹 Plugin을 사용하여 flutter와 ios widget 연결하기

### 1. Data 저장하기

main.dart → home_widget.dart → SwiftHomeWidgetPlugin.swift

```dart
//main.dart
Future<void> _sendData() async {
    try {
      return Future.wait([
        HomeWidget.saveWidgetData<String>('temperature', temperature),
        HomeWidget.saveWidgetData<String>('description', description),
        HomeWidget.saveWidgetData<String>('rainFall', rainFall),
        HomeWidget.saveWidgetData<String>('location', location),
      ]);
    } on PlatformException catch (exception) {
      debugPrint('Error Sending Data. $exception');
    }
  }

//home_widget.dart
static Future<bool?> saveWidgetData<T>(String id, T? data) {
    return _channel.invokeMethod<bool>('saveWidgetData', {
      'id': id,
      'data': data,
    });
  }

//SwiftHomeWidgetPlugin.swift
else if call.method == "saveWidgetData" {
            if (SwiftHomeWidgetPlugin.groupId == nil) {
                result(notInitializedError)
                return
            }
            guard let args = call.arguments else {
                return
            }
            if let myArgs = args as? [String: Any],
               let id = myArgs["id"] as? String,
               let data = myArgs["data"] {
                let preferences = UserDefaults.init(suiteName: SwiftHomeWidgetPlugin.groupId)
                preferences?.setValue(data, forKey: id)
                result(preferences?.synchronize() == true)
            } else {
                result(FlutterError(code: "-1", message: "InvalidArguments saveWidgetData must be called with id and data", details: nil))
            }
        }
```

- main.dart에서 web scraper를 사용하여 받아온 데이터를 swift plugin에 보내준다.
- preference.setValue
: SwiftHomeWidgetPlugin에서 받아온 데이터를 setValue를 사용하여 key값과 data를 저장한다.

### 2. 저장된 Data를 widget에서 사용하기

HomeWidgetExample.swift에서 widget을 만들기 위해 사용한다.

```dart
let data = UserDefaults.init(suiteName:widgetGroupId)
let entry = ExampleEntry(date: Date(), temperature: data?.string(forKey: "temperature") ?? "No temperature Set", description: data?.string(forKey: "description") ?? "No description Set", rainFall: data?.string(forKey: "rainFall") ?? "No rainFall Set", location: data?.string(forKey: "location") ?? "No location Set")
        completion(entry)
```

- 매칭된 GroupId에 있는 data를 가져와서 사용한다.

### 3. widget에 저장되어 있는 Data를 Flutter로 가져오기

main.dart → home_widget.dart → SwiftHomeWidgetPlugin.swift

```dart
//main.dart
Future<void> _loadData() async {
    try {
      return Future.wait([
        HomeWidget.getWidgetData<String>('temperature',
            defaultValue: 'Default temperature')
            .then((value) =>
            setState(() {
              temperature = value;
            })),
        HomeWidget.getWidgetData<String>('description',
            defaultValue: 'Default description')
            .then((value) =>
            setState(() {
              description = value;
            })),
        HomeWidget.getWidgetData<String>('rainFall',
            defaultValue: 'Default rainFall')
            .then((value) =>
            setState(() {
              rainFall = value;
            })),
        HomeWidget.getWidgetData<String>('location',
            defaultValue: 'Default location')
            .then((value) =>
            setState(() {
              location = value;
            })),
      ]);
    } on PlatformException catch (exception) {
      debugPrint('Error Getting Data. $exception');
    }
  }

//home_widget.dart
static Future<T?> getWidgetData<T>(String id, {T? defaultValue}) {
    return _channel.invokeMethod<T>('getWidgetData', {
      'id': id,
      'defaultValue': defaultValue,
    });
  }

//SwiftHomeWidgetPlugin.swift
else if call.method == "getWidgetData" {
            if (SwiftHomeWidgetPlugin.groupId == nil) {
                result(notInitializedError)
                return
            }
            guard let args = call.arguments else {
                return
            }
            if let myArgs = args as? [String: Any],
               let id = myArgs["id"] as? String,
               let defaultValue = myArgs["defaultValue"] {
                let preferences = UserDefaults.init(suiteName: SwiftHomeWidgetPlugin.groupId)
                result(preferences?.value(forKey: id) ?? defaultValue)
            } else {
                result(FlutterError(code: "-2", message: "InvalidArguments getWidgetData must be called with id", details: nil))
            }
        }
```

- call.arguments를 사용하여 swift native의 Data에 접근할 수 있다.

# 🔹 결과

- **ios home widget**

- **실제 data (naver weather)**

![%5BPlugin%5D%20IOS%20widget%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5%20ff8040eb979f4f3fa6265c96be0b0b07/Untitled%202.png](%5BPlugin%5D%20IOS%20widget%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5%20ff8040eb979f4f3fa6265c96be0b0b07/Untitled%202.png)

![%5BPlugin%5D%20IOS%20widget%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5%20ff8040eb979f4f3fa6265c96be0b0b07/Untitled%203.png](%5BPlugin%5D%20IOS%20widget%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5%20ff8040eb979f4f3fa6265c96be0b0b07/Untitled%203.png)

# 🔹 출처

[https://eunjin3786.tistory.com/212](https://eunjin3786.tistory.com/212)

[https://developer.apple.com/documentation/widgetkit/creating-a-widget-extension](https://developer.apple.com/documentation/widgetkit/creating-a-widget-extension)

[https://wonjerry.github.io/2019/11/13/make-flutter-plugin-part2/](https://wonjerry.github.io/2019/11/13/make-flutter-plugin-part2/)

[https://www.youtube.com/watch?v=yQh3p50W3b0](https://www.youtube.com/watch?v=yQh3p50W3b0)