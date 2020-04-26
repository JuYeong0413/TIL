## main.dart
```dart
import 'package:flutter/material.dart';
```
material library를 반드시 가져와야만 flutter framework, sdk에 포함된 모든 기본 widget과 material design 테마 요소들을 사용할 수 있음  
`i` -> `fm` 으로 빠르게 입력 가능  


```dart
void main() => runApp(MyApp());
```
- `void` : `main` 함수의 실행이 끝났을 때 아무런 값도 반환하지 않는다.  
- `main` 함수 : app의 시작점, 컴파일러가 코드를 컴퓨터가 이해할 수 있는 언어로 바꾸는 작업을 할 때 가장 먼저 `main` 함수를 참조한다.  
- `=>` : fat arrow, 코딩을 좀 더 간결하게 하기 위한 기호, `main` 함수가 다른 함수를 호출한다는 의미로 사용  
Flutter에서 최상위에 위치한 `runApp` 함수 호출, 최상위 함수이기 때문에 한 번만 호출한다.  
- `runApp` 함수 : 반드시 widget을 argument로 가져야 한다.  
- `MyApp` : widget tree에서 최상위에 위치하는 widget이고 screen layout을 최초로 build하는 역할을 한다.  


```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'First app',
      theme: ThemeData(
        primarySwatch: Colors.blue
      ),
      home: MyHomePage(),
    );
  }
}
```
- 최상위 widget이 될 `MyApp` 위젯은 최초로 app의 layout을 build 하는 역할  
즉, 뼈대를 만드는 역할만을 하기 때문에 어떤 변화나 움직임이 없는 정적인 위젯이 될 것이다. (stateless widget)  
- 모든 custom widget은 또 다른 widget을 리턴하는 `build` 함수를 가지고 있다.  
- `MaterialApp` : Flutter material library를 사용할 수 있는 기능을 가진 widget, 실질적으로 모든 widget을 감싸고 있다.  

- `ThemeData()` : app의 기본적인 디자인 테마를 지정하는 곳  
- `primarySwatch` : app에서 기본적으로 사용할 색상 견본(특정 색의 음영들을 기본 색상으로 지정해서 사용하겠다는 의미)  

- closing label : 위젯의 끝을 알려줌

- `home` : app이 정상적으로 실행되었을 때 가장 먼저 화면에 보여주는 경로  


```dart
class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First app'),
      ),
      body: Center(
        child: Column(
          children: <Widget>[
            Text('Hello'),
            Text('Hello'),
            Text('Hello')
          ],
        ),
      ),
    );
  }
}
```
- `Scaffold` : 앱 화면에 다양한 요소들을 배치하고 그릴 수 있도록 도와주는 빈 도화지같은 역할을 함  
만약 `Scaffold` 위젯이 없으면 어떠한 요소들도 앱 화면에 배치할 수 없다.  
- `MaterialApp`의 `title` : app을 총칭하는 이름  
- `AppBar` widget 내의 `title` : 앱 화면 `AppBar`에 출력되는 `title`  
- `body` : 본격적으로 앱 화면을 만드는 시작점, `Scaffold` widget 내에서 가장 중요한 요소  
- `Column` : 자신의 widget 내의 모든 요소를 세로로 배치함, `[]` 안에 세로로 정렬될 widgets을 나열  
