# 빠른 입출력
```python
input()
```
대신
```python
sys.stdin.readline()
```
사용하기. 대신 개행문자까지 입력받으므로 문자열 저장 시
```python
sys.stdin.readline().rstrip()
```
해 줄 것

