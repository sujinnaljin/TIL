# pointfree style

- 함수 이용할때 파라미터 이용하지 않고 함수 자체 이용하는 것

   ```javascript
  const mySpecialFunc1 = compose(
    (num) => inc(num),
    (num) => negate(num),
    (num1, num2) => pow(num1, num2)
  );
  
  const mySpecialFunc2 = compose(inc, negate, pow);
   ```

  

# 출처

- [FP in JS (자바스크립트로 접해보는 함수형 프로그래밍) - 함수 컴포지션, 커링](https://velog.io/@nakta/FP-in-JS-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A1%9C-%EC%A0%91%ED%95%B4%EB%B3%B4%EB%8A%94-%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%ED%95%A8%EC%88%98-%EC%BB%B4%ED%8F%AC%EC%A7%80%EC%85%98-%EC%BB%A4%EB%A7%81-s7k2z039vb)

