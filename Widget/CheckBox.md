# CheckBox

태그: Mobile-Midterm

# ✔️ Value & onChanged 필요하다.!

1. value
2. onChanged

```dart
Checkbox(
	value: todoController.todos[index].isCompleted,
	onChanged: (value) {
		todoController.changeStatus(todoController.todos[index]);
	},
),
```