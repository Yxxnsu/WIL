# [Widget] DataPicker

Tag: Widget
속성: 완료

# ✔️ DatePicker

달력을 통해서 시간을 쉽게 고를 수 있는 Widget

## ✔️ Code

```dart
DateTime _checkInDate = DateTime.now();
TimeOfDay _time = TimeOfDay.now();

Future<Null> _selectDate(BuildContext context) async {
    final DateTime picked = await showDatePicker(
        context: context,
        initialDate: _checkInDate,
        firstDate: DateTime(2021),
        lastDate: DateTime(2110));
    if (picked != null && picked != _checkInDate)
      setState(() {
        _checkInDate = picked;
      });
  }

## 출력
- DateFormat("yyyy-M-dd (EEE)").format(_checkInDate),
```