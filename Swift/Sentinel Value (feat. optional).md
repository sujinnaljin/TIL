# Sentinel Value (feat. optional)

- **특정한 상태**를 나타내는 value. (값이 없음을 나타낸다든가..)
- `var errorCode = 0` 이 성공 상황일때, 이때 `0` 이 sentinel value

- 하지만 추후 서버의 변경에 따라  `0` 이 에러를 나타낼 수도 있음  -> 에러 리턴 값에 대한 문서 없이는 이게 정상인지, 에러인지 **헷갈릴 수** 있음 
- 따라서 **value의 부재를 나타낼 수 있는 특정 type**이 있으면 좋음 -> **`nil` 의 등장**

# 출처

- [Part 3— iOS Interview Preparation Questions and Explanations](https://mdcode2021.medium.com/part-3-ios-interview-preparation-questions-and-explanations-e6fc5e391351)
- [Optionals](https://www.raywenderlich.com/books/swift-apprentice/v6.0/chapters/6-optionals)

