# 애플 M1 맥이 그렇게 빠른 이유 (feat. CISC vs RISC)

## TL; DR

### CISC (Complex Instruction Set Computer)

- 인텔 기반.
- **instruction** 마다 **길이가 다름**

### RISK (Reduced Instruction Set Computer)

- ARM 기반. m1에서 사용
- **instruction** 마다 **길이가 같음**

### RISK가 CISK 보다 빠른 이유

- **CPU 의 속도를 증가**시키기 위한 방법으로는 각각의 코어가 **동시에 여러개의 instruction을 수행**할 수 있게 만드는 것. 이를 위해선 **instruction 버퍼가 채워져야** 함.
- **instruction 버퍼를 빠르게 채울 수 있는 능력**은 **기계어**(machine code - instruction)를 빠르게 **micro-ops로 쪼갤 수 있는 능력**에 달려 있는데, 이러한 일을 수행하는 하드웨어 유닛을 `decoders` 라고 부름.
- **RISC** 의 **instruction은 길이가 같**기 때문에 **decoder에 바로 바로 채워넣을 수 있**으므로 빠름 (CISK는 instruction 분석 후 길이에 따라 decoder에 채워넣어줘야 함)



## '애플 M1 맥이 그렇게 빠른 이유' 요약본

### CPU 속도 증가를 달성하는 방법

아래 두가지 전략을 조합하여 달성할 수 있음

1. 연속된 instruction을 더 빨리 순차적으로 수행 -> 오늘날에는 클럭 스피드를 높이는것이 거의 불가능해짐. (무어의 법칙의 끝)
2. **동시에 다량의 insruction을 병렬 수행** -> **현대에는 여기에 집중**하며, 이에 대하여 두가지 접근법이 있음

   1. Multi-core - CPU 코어 갯수를 더 늘리자. 각각의 코어는 독립적이고 병렬적으로 일한다.
   2. **Out-of-Order processors** -각각의 **CPU 코어가 동시에 여러개의 instruction을 수행**할 수 있게 만들자.

### 비 순차적 실행 (Out-of-Order Execution) 이 동작하는 방법

- 더 강력한 성능의 코어를 만들기 위해, **하나의 코어가 더 많은 수의 instruction을 병렬적으로 수행**하게 만들 필요가 있습니다. [Our-of-Order execution](https://en.wikipedia.org/wiki/Out-of-order_execution) (OoOE)은 멀티 쓰레딩과 같은 기법 없이 **동시에 여러 instruction을 수행**하는 한가지 방법입니다.
- 데이터는 우리가 **데이터 버스(databus)**라고 부르는 것에 의해 여기저기로 보내 집니다. 이는 **메모리와 CPU의 여러 부분에 연결되어 데이터가 옮겨지는 길이나 파이프** 정도로 생각할 수 있습니다. 실제로 데이터버스는 그냥 전기를 전도하는 **구리 선에 불과**합니다. 데이터버스가 **충분히 넓으면 동시에 여러 바이트의 데이터**를 보낼 수 있습니다. 이를 통해 CPU는 실행해야 할 **instruction들을 동시에 왕창 buffer에 받을 수 있**습니다. 
- 하지만 그 **instruction**들은 하나하나 **순차적으로 실행되도록 작성**되어 있지요. 현대적인 microprocessor들은 **Out-of-Order execution (OoOE)라고 불리는 것을 수행**합니다. 즉, 현재 CPU에 불러와져 있는 **insturction들이 저장되어 있는 buffer를 보며, 어느 것이 어디에 의존성**을 가지고 있는지를 빠르게 분석합니다. (CPU는 각 instruction의 인풋이 하나 이상의 다른 instruction의 아웃풋에 의존하는가?). 
- 이러한 **관계들을 연결**하여 아주 길고 복잡한 **그래프**를 만들 수 있고, CPU는 그래프의 노드들을 분석하여 **어느 instruction이 병렬 수행** 될 수 있고, **어느 지점**에서 의존하고 있는 여러개의 연산 결과를 **기다려야 하는지를 결정**할 수 있습니다.

### M1의 비 순차 실행이 빠른 이유

- M1에 탑재되어 있는 Firestorm의 비 순차 실행은 아마 인텔과 AMD가 절대 따라잡을 수 없을 정도로 강력한데, 왜 그런지를 이해하기 위해 우리는 좀 더 기술적으로 깊게 들어가야 할 필요가 있습니다.
- **instruction 버퍼를 빠르게 채울 수 있는 능력**은 **기계어(machine code)를 빠르게 micro-ops로 쪼갤 수 있는 능력**에 달려 있습니다. 이러한 일을 수행하는 **하드웨어 유닛을 `decoders`** 라고 부릅니다.
- **인텔과 AMD**의 가장 크고 빠릿한 microprocessor는 열심히 **instruction을 micro-ops로 쪼개는** 총 **4개의 decoder**를 가지고 있습니다.
- **M1**은 무려 **8개**를 가지고 있는데, 즉 **instruction buffer를 훨씬 더 빨리 채울 수 있음**을 의미합니다.
- M1 Firestorm이 ARM RISC 아키텍처를 채택한게 중요해 지는 순간입니다. 아시다 시피, **x86 instruction**은 **1-15 바이트 사이의 임의의 길이**를 가질 수 있습니다. **RISC instruction**은 **고정된 길이**를 가집니다. 모든 ARM instruction은 **4바이트** 길이입니다.
- **모든 instruction들이 같은 길이**를 가지는 경우, 연속된 bytes 스트림을 쪼개어 8개의 **decoder들에게 병렬적으로 제공**하는것은 어렵지 않습니다.
- 반면 **x86** CPU의 경우, **decoder는 어디가 다음 instruction이 시작하는 위치인지를 알 방법이 없습**니다. 각 **instruction을 분석해서 그 길이가 얼마나 되는지를 실제로 분석**하는 수 밖에 없지요. decoder를 더 추가하는것은 다른 수많은 문제들을 야기하기 때문에, AMD에 따르면 자기들은 사실상 4개가 최대 한계치라고 합니다.
- 이것이 바로 M1 Firestorm 코어가 AMD와 인텔 CPU와 **같은 클럭 스피드**인데도 **두배**나 많은 instruction을 처리할 수 있는 이유입니다.



# 출처

- [애플 M1 맥이 그렇게 빠른 이유](https://lunatk.github.io/2020/12/14/20201214-why-is-apple-m1-chip-so-fast/)
