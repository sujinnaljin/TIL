# Subscript

- 콜렉션, 리스트, 시퀀스 등 **집합의 특정 member elements에 간단하게 접근할 수 있는 문법**

- array[index]처럼 index로 배열의 항목과, dictionary[key]처럼 key로 딕셔너리 항목에 접근하는 것

- 추가적인 메소드 없이 특정 값을 할당하거나 가져올 수 있다.

- 형태는 아래와 같다

  ```swift
  //1. 기본 형태
  subscript(index: Int) -> Int {
   get {
    // 반환 값
   }
   set(newValue) {
    // set 액션
   }
  }
  
  //2. 읽기 전용으로 선언 -> get으로 동작
  subscript(index: Int) -> Int {
   // 반환 값
  }
  ```



# 출처

- [Part 1 — iOS Interview Preparation Questions and Explanations](https://medium.com/swlh/part-1-ios-interview-preparation-questions-and-solutions-9b502dad8b63)
- [오늘의 Swift 상식 (Subscript)](https://medium.com/@jgj455/%EC%98%A4%EB%8A%98%EC%9D%98-swift-%EC%83%81%EC%8B%9D-subscript-2288551588f9)

