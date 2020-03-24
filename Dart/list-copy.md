# List Copy
```dart
List<String> originalList = ['a', 'b', 'c', 'd', 'e'];
List<String> newList;
```
위와 같이 `String` type인 두 개의 `List`가 존재할 때, `originalList`의 elements를 `newList`로 복사를 하고싶은 경우

## Shallow Copy
```dart
newList = originalList;

print(originalList);
print(newList);
```
를 출력해보면
```
[a, b, c, d, e]
[a, b, c, d, e]
```
`originalList`의 elements가 `newList`로 잘 복사된 것처럼 보인다.  
하지만 이 방법은 `object`의 elements가 아닌 `reference`를 복사하기 때문에,
```dart
originalList.add('f');
```
위와 같이 `originalList`에다가만 `'f'`를 추가하거나
```dart
newList.add('f');
```
`newList`에다가만 `'f'`를 추가한 결과를 출력해보면
```dart
[a, b, c, d, e, f]
[a, b, c, d, e, f]
```
의도했던 결과와 다르게 `originalList`와 `newList` 둘 다 `'f'`라는 element가 추가되는 결과가 나온다.  
이외에 아래와 같은 방법으로 복사하는 것도 `shallow copy`가 되어 `originalList`와 `newList` 둘 다 변경시킨다.
```dart
List<String> newList = [];
newList.addAll(originalList);
```
```dart
newList = List.from(originalList);
```

## Deep Copy
동일한 `List`에 `reference`를 가지는 것이 아니라 `deep copy`를 통해 elements 자체를 복사하려면
```dart
for(int i = 0; i < originalList.length; i++) {
  newList = List<String>.generate(originalList.length,(i) => originalList[i]);
}
```
위와 같이 `generate`를 이용해야 한다.
```dart
print(originalList);
print(newList);

newList.add('f');

print('--------------------');
print(originalList);
print(newList);
```
`deep copy`의 결과를 출력해보면
```
[a, b, c, d, e]
[a, b, c, d, e]
--------------------
[a, b, c, d, e]
[a, b, c, d, e, f]
```
처음에 의도했던 것처럼 `newList`에만 `'f'`가 추가된 것을 확인할 수 있다.
