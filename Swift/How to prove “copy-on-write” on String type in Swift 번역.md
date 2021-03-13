# How to prove “copy-on-write” on String type in Swift 번역

Compiler는 "Hello World"와 "Hello" + "Word"에 대해 같은 storage 를 제공합니다. 아래 코드의 결과물 확인을 통해 실제로 증명할 수 있습니다.

```
swiftc -emit-assembly cow.swift
```

오직 하나의 string literal을 정의하고 있습니다.

```
    .section    __TEXT,__cstring,cstring_literals
L___unnamed_1:
    .asciz  "Hello World"
```

string이 변경되는 즉시, string 저장 buffer (tuple 에서 첫번째 멤버 - String의 baseAddress)의 주소가 달라집니다.

> 참고로 baseAddress는 UnsafeRawBufferPointer 나 UnsafeMutableRawBufferPointer같은데 있는 포인터인가봄. pointer to the first byte of the buffer. (UnsafeMutableRawBufferPointer.swift.gyb 에 정의)

tuple의 두번째 멤버는 string buffer의 owner의 pointer 입니다. buffer는 동적으로 할당될때 owner를 갖게 됩니다. owner은 buffer의 reference counting을 신경쓰는 class instance 입니다. owner이 deallocated 되면, buffer은 relased 됩니다.

native storage 아래에는, 하나의 `ManagedBuffer` 인스턴스가 dynamic string buffer로 사용되고 있을거라고 생각합니다. 그래서 raw buffer pointer (tuple 에서 첫번째 element) 는 해당 instance body 의 시작 부분을 point 하고 owner은 header의 시작부분을 point 할 것 입니다.

```swift
func _rawIdentifier(s: String) -> (UInt, UInt) {
    //Returns the bits of the given instance, interpreted as having the specified type.
    let tripe = unsafeBitCast(s, to: (UInt, UInt).self)
    let minusCount = (tripe.0, tripe.1)
    return minusCount
}

var xString = "Hello World"
var yString = "Hello" + " World"


// These two rawIdentifiers were the same
_rawIdentifier(s: xString) //(.0 8022916924116329800, .1 16933534598919646322)
_rawIdentifier(s: yString) //(.0 8022916924116329800, .1 16933534598919646322)

yString = "hihi"
_rawIdentifier(s: xString) //(.0 8022916924116329800, .1 16933534598919646322)
_rawIdentifier(s: yString) //(.0 1768450408, .1 16429131440647569408)
```

----



```swift
func address(of object: UnsafeRawPointer) -> String
```

이 함수가 array에 대해서 같은 값을 return 하면서 String에 대해서 그렇지 않은 이유는 아래와 같습니다.

unsafe pointer를 인자로 받는 함수에 array를 전달하는 것은 첫번째 어레이 인자의 주소를 전달하는 것입니다. 따라서 array들이 같은 저장공간을 사용한다면 동일한 값이 나오게 됩니다.

반면 unsafe pointer를 인자로 받는 함수에 string을 전달하는 것은 string을 나타내는 임시 UTF-8 의 pointer를 전달하는 것입니다. 이 주소는 같은 string일지라도 매 호출마다 달라질 수 있습니다.

이러한 동작은 `UnsafePointer<T>` 인자에 대한 "Using Swift with Cocoa and Objective-C" 레퍼런스에 설명되어 있으며, 이는 `UnsafeRawPointer` 인자에 대해서도 동일하게 동작합니다.



# 출처

[How to prove “copy-on-write” on String type in Swift](https://stackoverflow.com/questions/46747363/how-to-prove-copy-on-write-on-string-type-in-swift)

