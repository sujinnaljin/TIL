# 커링(Currying)

-  파라미터 채우지 않는 한 함수로 남아있는 것
-  다중 인수를 갖는 함수를 단일 인수로 갖는 함수들의 함수열로 바꾸는 것
-  즉, 여러 개의 인자를 가진 함수를 호출할 경우, 파라미터의 수보다 적은 수의 파라미터를 인자로 받으면 누락된 파라미터를 인자로 받는 함수를 리턴하는 기능
-  커링을 사용하게 될 경우 다인수 함수를 단항 함수로 바꾸어 사용할 수 있게 되므로 코드의 복잡도를 낮추게 된다. 쉽게 말해 5개의 인자를 받는 함수를 한개의 인자씩 전달하도록 구현할 수 있게 된다. 5개의 파라미터를 동시에 던지는 것 보단 한개씩 던질 경우 가독성이 훨씬 올라가게 된다.

![img](https://uzilog-upload.s3.ap-northeast-2.amazonaws.com/private/ap-northeast-2%3Ab6c10628-1f45-492c-a9eb-f54020bc8014/1582030225156-image.png)

# 출처

- [FP in JS (자바스크립트로 접해보는 함수형 프로그래밍) - 함수 컴포지션, 커링](https://velog.io/@nakta/FP-in-JS-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A1%9C-%EC%A0%91%ED%95%B4%EB%B3%B4%EB%8A%94-%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%ED%95%A8%EC%88%98-%EC%BB%B4%ED%8F%AC%EC%A7%80%EC%85%98-%EC%BB%A4%EB%A7%81-s7k2z039vb)
- [함수형 프로그래밍 2](https://uzihoon.com/post/8cfd1d30-5249-11ea-82a2-e3de4cde3cdc)
- [[Swift]커링(Currying)](http://minsone.github.io/programming/what-is-currying-in-swift)
