한 페이지에 여러 개의 `FloatingActionButton`이 존재하면 에러가 발생한다.  
```
flutter: ══╡ EXCEPTION CAUGHT BY SCHEDULER LIBRARY ╞═════════════════════════════════════════════════════════
flutter: The following assertion was thrown during a scheduler callback:
flutter: There are multiple heroes that share the same tag within a subtree.
flutter: Within each subtree for which heroes are to be animated (i.e. a PageRoute subtree), each Hero must
flutter: have a unique non-null tag.
flutter: In this case, multiple heroes had the following tag: <default FloatingActionButton tag>
flutter: ├# Here is the subtree for one of the offending heroes: Hero
```

`FloatingActionButton`의 `heroTag` 속성에 unique id를 부여해주거나, hero tag을 가지지 않길 원한다면 `null`로 설정하면 된다.  
