# 겹치게 find
```python
sentence = "xyyyzyyy"
pattern = "y\wy\wy"
```
일 때
```python
re.findall(pattern, sentence)
```
는 `yyyzy`만 찾는다.
`yzyyy`도 찾으려면 아래와 같이 `?=()`를 이용해서 `finditer`로 찾아야 한다.
```python
for m in re.finditer('(?=({}))'.format(pattern), sentence):
    print(m.start(), m.end(), m.group(1))
```
결과:
```
1 1 yyyzy
3 3 yzyyy
```
