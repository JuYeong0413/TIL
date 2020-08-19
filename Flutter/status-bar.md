# Status Bar 색상 변경  
```dart
import 'package:flutter/services.dart';
```

```dart
void main() {
  SystemChrome.setSystemUIOverlayStyle(SystemUiOverlayStyle.dark.copyWith(
    statusBarColor: Colors.transparent,
    statusBarIconBrightness: Brightness.dark
  ));
  runApp(MyApp());
}
```
Flutter app의 초기 status bar color은 하얀색이어서 검정색으로 변경하고 싶어서 위 코드를 추가했다.  
그런데 첫 페이지만 검정색으로 나오고 `BottomNavigationBar`로 다른 페이지로 이동하면 다시 하얀색으로 변하는 현상이 있었다.  

`AppBar`에 `brightness: Brightness.light`를 추가하니 해결됐다.  
`brightness: Brightness.light`는 status bar text color를 `black`으로 만들고, `brightness: Brightness.dartk`는 status bar text color를 white로 만든다고 한다. [Stack Overflow 참고자료](https://stackoverflow.com/a/51710612)  

# Status Bar 높이 구하기  
```dart
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final double statusBarHeight = MediaQuery.of(context).padding.top;
    
    return Scaffold(
      ...
    );
  }
}
```
