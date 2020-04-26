## 클래스
객체가 가져야 하는 속성과 기능을 정의한 내용을 담고 있는 설계도 역할  

## 객체
클래스가 정의된 후 메모리 상에 할당되었을 때 이를 객체라고 함  

## 인스턴스
클래스를 기반으로 생성됨  
클래스의 속성과 기능을 똑같이 가지고 있고 프로그래밍 상에서 사용되는 대상

---

```dart
class Person {
  // 객체들의 정보 정의
  String name;
  int age;
  String sex;
  
  Person({String name, int age, String sex}) { // named argument
    this.name = name;
    this.age = age;
    this.sex = sex;
  }
}

addNumber(int num1, int num2) {
  // 함수의 body, 함수가 실행할 기능을 정리
  return num1 + num2;
}

void main() {
  Person p1 = new Person(age: 30); // 인스턴스 생성
  Person p2 = new Person(sex: 'male');
  
  print(p1.age);
  print(p2.sex);
  
  addNumber(3, 4);
  print(addNumber(3, 4));
}
```
