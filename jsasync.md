![image](https://t1.daumcdn.net/cfile/tistory/99A7234F5C321A7F2B)

## call stack

- 자바스크립트는 단일스레드 프로그래밍 언어이므로 단일 호출 스택을 가짐

- 함수를 실행하면 해당 함수의 기록을 스택 맨 위에 추가

- c언어의 호출 스택과 같아보인다.

## event queue

- 처리할 메세지 목록과 실행할 콜백 함수들의 리스트이다. 버튼 클릭이나 DOM 이벤트, http 요청, setTimeout 같은 비동기 함수는 Web API를 호출하며, 이를 event queue에 넣는다.

- event queue는 대기하다가 stack이 텅 비는 시점에 event loop를 돌려 해당 비동기 함수를 스택에 넣는다.

## event loop
![img](https://miro.medium.com/max/478/1*KGBiAxjeD9JT2j6KDo0zUg.png)

- call stack과 event queue를 감시하는 역할

- event loop는 event queue에서 첫 번째 이벤트를 가져다가 event queue에 밀어 넣어, 결과적으로는 해당 이벤트가 실행

- 이러한 반복을 event loop에서는 틱(tick)이라고 부름.

- 각각의 이벤트는 단순히 함수를 실행

#### 예제를 보면서 이해하자!!

```javascript
console.log('hi');
setTimeout(function f() {
    console.log('bye');
}, 5000);
console.log('bye bye');
```

1. console.log('hi')가 call stack에 추가
![image](https://miro.medium.com/max/700/1*dvrghQCVQIZOfNC27Jrtlw.png)
2. console.log('hi')가 실행
![image](https://miro.medium.com/max/700/1*yn9Y4PXNP8XTz6mtCAzDZQ.png)
3. console.log('hi')가 call stack에서 제거
![image](https://miro.medium.com/max/700/1*iBedryNbqtixYTKviPC1tA.png)
4. setTimeout(function -)가 콜스택에 추가
![image](https://miro.medium.com/max/700/1*HIn-BxIP38X6mF_65snMKg.png)
5. setTimeout()이 실행. 브라우저가 웹 api의 일환인 타이머를 생성하고, 이 타이머가 카운트다운을 처리
![image](https://miro.medium.com/max/700/1*vd3X2O_qRfqaEpW4AfZM4w.png)
6. setTimeout()이 완료되고 콜스택에서 제거
![image](https://miro.medium.com/max/700/1*_nYLhoZPKD_HPhpJtQeErA.png)
7. console.log('bye')가 콜스택에 추가
![image](https://miro.medium.com/max/700/1*1NAeDnEv6DWFewX_C-L8mg.png)
8. console.log('bye')가 실행
![image](https://miro.medium.com/max/700/1*UwtM7DmK1BmlBOUUYEopGQ.png)
9. console.log('bye')가 콜스택에서 제거
![image](https://miro.medium.com/max/700/1*-vHNuJsJVXvqq5dLHPt7cQ.png)
10. 최소 5초가 지난 다음에 타이머가 완료되고 cb1을 event queue에 넣는다.
![image](https://miro.medium.com/max/700/1*eOj6NVwGI2N78onh6CuCbA.png)
11. 이벤트루프는 cb1을 event queue에서 가져다가 call stack에 밀어넣는다.
![image](https://miro.medium.com/max/700/1*jQMQ9BEKPycs2wFC233aNg.png)
12. cb1이 실행되고 console.log('cb1')이 call stack에 추가된다.
![image](https://miro.medium.com/max/700/1*hpyVeL1zsaeHaqS7mU4Qfw.png)
13. console.log('cb1')이 실행된다.
![image](https://miro.medium.com/max/700/1*lvOtCg75ObmUTOxIS6anEQ.png)
14. console.log('cb1')이 callstack에서 제거된다.
![image](https://miro.medium.com/max/700/1*Jyyot22aRkKMF3LN1bgE-w.png)
15. cb1이 call stack에서 제거된다.
![image](https://miro.medium.com/max/700/1*t2Btfb_tBbBxTvyVgKX0Qg.png)
