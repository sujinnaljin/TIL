# Carthage 빌드 in Xcode 12
- Swift버전이 새롭게 업데이트 되면 Carthage를 사용하는 framework의 binary를 다시 빌드해야한다.
- 원래는 `Cartfile` 파일이 있는 폴더 경로로 가서 `carthage update` 명령어를 실행시키면 된다.
- 하지만 Xcode 12에서 arm64(apple silicon) 설정으로 빌드를 하다보니 오류가 생긴다. 우리는 인텔 맥을 사용하고 있으니 x86_64로 빌드를 해야 되기 때문.
- 따라서  shell script 를 이용해 강제로 x86_64 로 셋팅해서 빌드 해야한다.
- `carthage-build.sh` 파일에 아래와 같은 내용을 작성하고, `./carthage-build.sh --platform iOS` 명령어를 실행시켜주면 된다.

```sh
#!/usr/bin/env bash

# carthage-build.sh
# Usage example: ./carthage-build.sh --platform iOS

set -euo pipefail

xcconfig=$(mktemp /tmp/static.xcconfig.XXXXXX)
trap 'rm -f "$xcconfig"' INT TERM HUP EXIT

# For Xcode 12 make sure EXCLUDED_ARCHS is set to arm architectures otherwise
# the build will fail on lipo due to duplicate architectures.

CURRENT_XCODE_VERSION=$(xcodebuild -version | grep "Build version" | cut -d' ' -f3)

echo "EXCLUDED_ARCHS__EFFECTIVE_PLATFORM_SUFFIX_simulator__NATIVE_ARCH_64_BIT_x86_64__XCODE_1200__BUILD_$CURRENT_XCODE_VERSION = arm64 arm64e armv7 armv7s armv6 armv8" >> $xcconfig
echo 'EXCLUDED_ARCHS__EFFECTIVE_PLATFORM_SUFFIX_simulator__NATIVE_ARCH_64_BIT_x86_64__XCODE_1200 = $(EXCLUDED_ARCHS__EFFECTIVE_PLATFORM_SUFFIX_simulator__NATIVE_ARCH_64_BIT_x86_64__XCODE_1200__BUILD_$(XCODE_PRODUCT_BUILD_VERSION))' >> $xcconfig
echo 'EXCLUDED_ARCHS = $(inherited) $(EXCLUDED_ARCHS__EFFECTIVE_PLATFORM_SUFFIX_$(EFFECTIVE_PLATFORM_SUFFIX)__NATIVE_ARCH_64_BIT_$(NATIVE_ARCH_64_BIT)__XCODE_$(XCODE_VERSION_MAJOR))' >> $xcconfig

# echo 'IPHONEOS_DEPLOYMENT_TARGET=13.0' >> $xcconfig
# echo 'SWIFT_TREAT_WARNINGS_AS_ERRORS=NO' >> $xcconfig
# echo 'GCC_TREAT_WARNINGS_AS_ERRORS=NO' >> $xcconfig

export XCODE_XCCONFIG_FILE="$xcconfig"
carthage build "$@"

```
- `Xcode > Preferences > Locations > Command Line Tools` 보고 스위프트 버전을 판단하기 때문에 원하는 빌드 버전을 이곳에서 우선 설정한 후 .sh 명령어를 실행시킨다

![image](https://user-images.githubusercontent.com/20410193/113525666-3abc2d80-95f1-11eb-98f3-2d9ca7f05556.png)

# 출처
- [[iOS] Carthage 로 RIBs 프로젝트 셋팅하기 #2](https://maart.tistory.com/82)
- [[iOS] Carthage 가 Xcode 12에서 빌드오류가 난다고!](https://maart.tistory.com/81)
- [Xcode 12 업데이트 후 Carthage 빌드가 되지 않는 현상](https://medium.com/@jhseo.dev/xcode-12-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8-%ED%9B%84-carthage-%EB%B9%8C%EB%93%9C%EA%B0%80-%EB%90%98%EC%A7%80-%EC%95%8A%EB%8A%94-%ED%98%84%EC%83%81-e74ea8d4bcf2)
- [[Xcode] Xcode12에서 시뮬레이터 빌드 오류 원인 및 해결방법](https://jusung.github.io/Xcode12-Build-Error/)
