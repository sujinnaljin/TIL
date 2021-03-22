# Size / Alignment / Stride

![Size, stride, and alignment summarized.](https://swiftunboxed.com/images/size-stride-alignment-summary.png)

## Size

- 모든 데이터에 도달하기 위해 포인터에서 읽어야 할 바이트 수

## Alignment

  - 각 인스턴스가 있어야하는 "균등하게 나눌 수있는" 숫자. (the the “evenly divisible by” number that each instance needs to be at)

  - 현대의 컴퓨터는 메모리에 데이터를 쓰거나 읽을 때 워드(WORD)라는 단위로 수행. 즉, 32Bit 컴퓨터에서는 메모리를 4바이트 단위로 접근해서 읽고 쓴다.

  - 그림처럼 property 값이 제한된 byte (여기선 4 byte) 내에 있을때 정렬되었다고 함.

    ![img](https://t1.daumcdn.net/cfile/tistory/2272894E57BED14A24)

  - 정렬을 보장하려면 구조 요소 사이 또는 구조의 마지막 요소 뒤에 패딩을 삽입해야 할 수 있다.

  - struct 의 alignment는 각 프로퍼티의 alignment 중 가장 큰 값.

### 예시

**예시 1** 

```swift
struct SampleStruct {
    let a: Bool //1
    let b: Int32 //4
    let c: Bool //1
}
MemoryLayout<SampleStruct>.alignment //4
MemoryLayout<SampleStruct>.size //9
MemoryLayout<SampleStruct>.stride //12
```
![image](https://user-images.githubusercontent.com/20410193/111945604-59b0bf00-8b1d-11eb-93b6-b961156802f0.png)


**예시 2**

예시 1에 비해 중간에 Bool 이 추가되었지만 size 는 같다. padding 이 줄어든 것 뿐

```swift
struct SampleStruct2 {
    let a: Bool //1
    let b: Bool //1
    let c: Int32 //4
    let d: Bool //1
}
MemoryLayout<SampleStruct2>.alignment //4
MemoryLayout<SampleStruct2>.size //9
MemoryLayout<SampleStruct2>.stride //12
```
![image](https://user-images.githubusercontent.com/20410193/111945623-61706380-8b1d-11eb-823d-776b5934f778.png)



**예시 3**

예시 2와 프로퍼티 타입들은 똑같지만 순서가 바뀌어서 size, stride 값이 달라진다

```swift
struct SampleStruct3 {
    let a: Int32 //4
    let b: Bool //1
    let c: Bool //1
    let d: Bool //1
}
MemoryLayout<SampleStruct3>.alignment //4
MemoryLayout<SampleStruct3>.size //7
MemoryLayout<SampleStruct3>.stride //8
```
![image](https://user-images.githubusercontent.com/20410193/111945034-47825100-8b1c-11eb-8817-f416a5267639.png)



## Stride

- 다음 element에 도달하기 위해 진행할 바이트 수. size 보다 크거나 같은 alignment 의 배수.

# 출처

- [구조체의 최적화 (구조체 정렬)](https://seohs.tistory.com/406)

- [Size, Stride, Alignment](https://swiftunboxed.com/internals/size-stride-alignment/)
