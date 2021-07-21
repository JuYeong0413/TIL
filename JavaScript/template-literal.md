# Template literal
템플릿 리터럴(`` ` ``)을 사용하던 도중 배열에 대해 `map()`으로 출력을 할 때 이상하게 쉼표가 함께 나오는 현상이 있었다.  
`Array.prototype.map()` 메소는 새로운 배열을 반환하는데, 템플릿 리터럴이 디폴트로 `,`로 배열을 합치는 `toString()`을 사용하기 때문이었다.  
그래서 문자열로 출력하려면 `.join('');` 을 붙여줘야 한다. 👉🏻 [Stackoverflow 답변](https://stackoverflow.com/a/45812277/10825831)  

