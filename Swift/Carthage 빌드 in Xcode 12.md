# Carthage 빌드 in Xcode 12
- Xcode 12에서 arm64 설정으로 빌드를 하다보니 오류가 생기는 것 -> shell script 를 이용해 강제로 x86_64 로 셋팅해서 빌드 필요
```
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
# 출처
- [[iOS] Carthage 로 RIBs 프로젝트 셋팅하기 #2](https://maart.tistory.com/82)
- [[iOS] Carthage 가 Xcode 12에서 빌드오류가 난다고!](https://maart.tistory.com/81)
