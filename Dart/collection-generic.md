# Collection
데이터들을 모아서 가지고 있는 자료구조

# Generic
Collection이 가지고 있는 데이터들의 데이터 타입을 지정

# List
### fixed-length list
```dart
var number = new List(5);
```
list 내의 데이터의 개수가 지정한 개수만큼만 올 수 있다.
### growable list
```dart
var number = new List();
```
개수에 제한이 없다.


```dart
int addNumber(int num1, int num2) {
  return num1 + num2;
}

void main() {
  List<dynamic> number = new List();
  
  number.add(2);
  number.add('test');
  number.add(7.4);
  number.add(addNumber(3,4));
  number.add(true);
  
  print(number);
}
```
