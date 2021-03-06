# Early Return
### Early Return을 사용하지 않은 코드
```python
for char in string:
    if char.isalpha():
        arr_index = ord(char) - ord('a')
        alphabet_occurrence_array[arr_index] += 1
```
### Early Return을 사용한 코드
```python
for char in string:
    if not char.isalpha():
        continue
    arr_index = ord(char) - ord('a')
    alphabet_occurrence_array[arr_index] += 1
```
코드의 가독성이 좋아지고, depth를 줄일 수 있다.  
