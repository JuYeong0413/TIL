# deque
```python
from collections import deque

queue = deque()
```
`deque`의 길이를 지정하려면  
```python
queue = deque(maxlen=5)
```
원소를 `maxlen`보다 많이 넣으면 자동으로 `maxlen`이 갱신됨  

## append
```python
element = 1
queue.append(element)
```

## remove
```python
queue.popleft()
```

## index
```python
first = queue[0]
last = queue[-1]
```
`list`처럼 접근 가능  

## length
```python
len(queue)
```
