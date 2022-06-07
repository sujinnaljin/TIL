# BezierPath 의 init(roundedRect:byRoundingCorners:cornerRadii:)

지정된 모서리가 둥근 직사각형 경로 가진 Bézier path 를 반환 

- `rect` - 경로(path)의 기본 모양을 정의하는 직사각형
- `corners` - 둥글게 할 모서리를 식별하는 비트 마스크 값. 이 매개변수를 사용하여 직사각형 모서리의 부분 집합만 둥글게 할 수 있음
- `cornerRadii` - 각 모서리 타원의 radius. 직사각형의 너비 또는 높이의 절반보다 큰 값은 너비 또는 높이의 절반으로 적절하게 고정됨

# 출처 

- https://developer.apple.com/documentation/uikit/uibezierpath/1624368-init
