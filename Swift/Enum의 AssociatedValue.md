# Enum의 AssociatedValue

- enum에서 경우에 따라 case 값과 함께 다른 type의 값들을 저장하는 것이 유용

- 이러한 추가 정보를 associated value 라고 함

- 다른 언어에서는 discriminated unions, tagged unions, 또는 variants 라고 부르는 것

```swift
  //정의
  enum Barcode {
      case upc(Int, Int, Int, Int)
      case qrCode(String)
  }
  
  
  //사용
  var productBarcode = Barcode.upc(8, 85909, 51226, 3)
  switch productBarcode {
  case .upc(let numberSystem, let manufacturer, let product, let check):
      print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
  case .qrCode(let productCode):
      print("QR code: \(productCode).")
  }
```



# 출처

- [https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html](https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html)

