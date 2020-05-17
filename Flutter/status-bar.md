# Status Bar 색상 변경
```dart
void main() {
  SystemChrome.setSystemUIOverlayStyle(SystemUiOverlayStyle.dark.copyWith(
    statusBarColor: Colors.transparent,
    statusBarIconBrightness: Brightness.dark
  ));
  runApp(MyApp());
}
```
