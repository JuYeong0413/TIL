# AppBar
## 자동으로 생성되는 back button 색상 변경하기
```dart
appBar: AppBar(
  iconTheme: IconThemeData(
    color: Colors.amber, // change color
  ),
  title: Text("AppBar"),
),
```

## back button icon 변경하기
```dart
appBar: AppBar(
  leading: IconButton(
    icon: Icon(Icons.arrow_back, color: Colors.grey[700]),
    onPressed: () => Navigator.of(context).pop(),
  ), 
  backgroundColor: Colors.transparent,
  elevation: 0.0,
),
```
