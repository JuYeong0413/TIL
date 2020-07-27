# 기기 OS 확인하는 방법  
```dart
import 'dart:io' show Platform;
```
안드로이드는
```dart
Platform.isAndroid
```
iOS는
```dart
Platform.isIOS
```
로 확인한다.

```dart
String os = Platform.operatingSystem;
```
와 같이 `String` 형태로 가져올 수도 있다.
