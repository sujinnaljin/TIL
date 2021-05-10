# reducer

- reducer는 현재 상태, 이벤트를 취하고 새로운 상태를 출력 하는 순수 함수 (어떤 함수에 동일한 인자를 주었을 때 **항상** 같은 값을 리턴하는 함수 \+ 외부의 상태를 변경하지 않는 함수)

- swift 에서는 Reactorkit에 reduce 함수 있다.

> ```
> func reduce(state: State, mutation: Mutation) -> State
> ```
>
> `reduce()` generates a new `State` from a previous `State` and a `Mutation`.
>
> This method is a pure function. It should just return a new `State` synchronously. Don't perform any side effects in this function.

# 출처

- [A DSL for state machines in Swift](https://twittemb.github.io/posts/2021-02-13-StateMachineDSL/)
- [순수 함수란? (함수형 프로그래밍의 뿌리, 함수의 부수효과를 없앤다)](https://jeong-pro.tistory.com/23)
- [ReactorKit](https://github.com/ReactorKit/ReactorKit)
