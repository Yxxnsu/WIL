# [Widget] ListTile

Tag: Widget
속성: 완료

# ✔️ ListTile

아이콘과 Text , SubTitle 등을 쉽게 표현해주는 좋은 위젯!

## ✨ 코드

```dart
	 ListTile(
      title: Text('Home'),
      leading: Icon(
          Icons.home,
          color: Colors.blue,
      ),
      onTap:(){
         Navigator.pushNamed(context, '/home');
      },
   ),
```