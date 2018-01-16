자바스크립 인라인 이벤트 핸들링은 안좋은 습관 (bad-practice) 이다.

이유는

- 나중에 수정 이슈가 생길때, 일일이 돔을 찾으면서 수정을 해줘야 한다.
- 정보인 HTML과 제어인 JavaScript가 혼재된 형태이기 때문에 바람직한 방법이라고 할수는 없다. 

HTML 안에 제어 로직을 넣는건 안좋다는걸 아는데 이런 습관을 사용하고 있었다..
제어로직은 항상 자바스크립트 안에 넣어두자 명심하자...

```
Using inline event handlers, YUCK!
Just don’t do it! Take the unobtrusive approach instead.

Big no-no! –

<a href="javascript:void doSomething();">Click</a>
<!--OR-->
<a href="#" onclick="return doSomething();">Click</a>
Much better:

jQuery('element').click(doSomething);
(I’m not saying you have to use jQuery but you should develop a couple of abstractions to make it easier to work with events and DOM elements – at the least)
```
