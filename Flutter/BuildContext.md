## BuildContext
widget tree에서 현재 widget의 위치를 알 수 있는 정보
`context`는 `BuildContext` 클래스의 인스턴스


## Scaffold.of(context) method
현재 주어진 context에서 위로 올라가면서 가장 가까운 `Scaffold`를 찾아서 반환하라.

## Something.of(context)
위로 거슬러 올라가면서 가장 가까운 Something을 찾아서 반환하라.

## Theme.of(context)
위로 거슬러 올라가면서 가장 가까운 theme을 찾아서 반환하라.
