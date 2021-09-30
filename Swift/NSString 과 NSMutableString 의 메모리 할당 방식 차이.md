# NSString 과 NSMutableString 의 메모리 할당 방식 차이

## NSString

- 모든 **object 할당**과 마찬가지로 **문자열 할당**도 **시간과 메모리** 모두에서 **비용**이 많이 소요 됨
- 런타임은 **성능을 향상**시키고 메모리 오버헤드를 줄이기 위해 문자열 리터럴을 인스턴스화하는 동안 약간의 **트릭**을 수행
  1. 런타임에 생성 할 문자열 개체 수를 줄이기 위해 **NSString 클래스는 문자열 풀을 유지**
  2. 코드가 문자열 리터럴을 만들 때마다 **런타임은 문자열 리터럴 풀을 먼저 확인** 후, 문자열이 풀에 **이미 있는 경우 풀링된 인스턴스에 대한 참조를 반환**
  3. 문자열이 **풀에 없으면 새 문자열 개체가 인스턴스화된 후 풀에 배치** 됨
- **string**은 **불변(immutable)** 하고 **데이터 손상 걱정 없이 공유**할 수 있기 때문에 Objective-C는 위와 같은 **최적화를 수행**할 수 있음
- 아래 예시에서는 두 문자열이 **빈 문자열로 초기**화 되기 때문에 **문자열 풀에서 동일한 문자열 참조**를 반환

```objective-c
NSString *myString1 = [NSString new];
NSString *myString2 = [NSString new];

NSLog(@"%p",myString3);0x10622e470
NSLog(@"%p",myString4);0x10622e470
```

## NSMutableString

- NSMutableString은 위와 같은 방식으로 작동하지 않음. 즉, **런타임은 풀을 만들지 않음** 
- NSMutableString은 **변경 가능**하고 문자열이 변경 가능한 경우 항상 **데이터가 손상될 우려**가 있기 때문

```objc
 NSMutableString *myString1 = [NSMutableString new];
 NSMutableString *myString2 = [NSMutableString new];

 NSLog(@"%p",myString1);0x00007fe572416da0
 NSLog(@"%p",myString2);0x00007fe5724114a0
```



# 출처

- [Memory allocation for NSString and NSMutableString](https://stackoverflow.com/questions/37045499/memory-allocation-for-nsstring-and-nsmutablestring)

