# Test Double

- 테스트를 진행하기 어려운 경우 이를 대신해 테스트를 진행할 수 있도록 만들어주는 객체

  <img width="597" alt="image" src="https://user-images.githubusercontent.com/20410193/174092463-cf13ee81-f63e-4cd9-8aca-0900a6265a93.png">


- 영화 산업이 주연 배우가 수행하기에 잠재적으로 위험하거나 위험한 무언가를 촬영하기를 원할 때, 그들은 장면에서 배우를 대신할 "스턴트 더블"을 고용했음

  스턴트 더블은 장면의 특정 요구 사항을 충족할 수 있는 고도로 훈련된 개인으로, 높은 곳에서 떨어지는 방법, 자동차 충돌 방법 또는 장면에서 요구하는 모든 방법을 알고 있음.

  **스턴트 더블**이 배우를 얼마나 닮아야 하는지는 장면의 특성에 따라 다르지만, 일반적으로 배우의 키를 막연히 닮은 사람이 그 자리를 대신할 수 있음

- 테스트 환경에서도 비슷하게 **테스트 대상 시스템 (SUT) 이 모종의 이유로 테스트하기 어려운 상황** (사용할 수 없거나 테스트에 필요한 결과를 반환하지 않거나 사이드 이펙트가 있는  요소에 의존할 때 등) 이 있을 수 있음

- 실제 종속 구성 요소(DOC)를  사용할 수 없는 (또는 사용하지 않기로 선택한) 테스트를 작성할 때 **테스트 더블로 대체** 가능

  테스트 목적을 위해 실제 [DOC](http://xunitpatterns.com/DOC.html) ( [SUT](http://xunitpatterns.com/SUT.html) 아님)를  을 스턴트 더블에 해당하는 테스트 더블 로 바꿀 수 있음

- Test Double은 실제 [DOC](http://xunitpatterns.com/DOC.html) 처럼 정확하게 동작할 필요 없고, SUT 가 실제 API 라고 생각 하도록 실제 API와 동일한 API를 제공하기만 하면 됨

  즉 SUT 과 테스트 더블이 상호 작용할 때, 실제 요소와 대화하고 있지 않다는 것을 인식하지 못하게 하되, 불가능한 테스트를 가능하게 만드는 것이 목표

- 사용하기로 선택한 Test Double 의 종류에 관계없이 [DOC](http://xunitpatterns.com/DOC.html) 의 전체 인터페이스를 구현할 필요가 없이 특정 테스트에 필요한 기능만 제공하면 됨. 

<img width="429" alt="image" src="https://user-images.githubusercontent.com/20410193/174092501-b906433d-0e9d-46df-b3db-5088e41e369b.png">


## Dummy

- 인스턴스화 된 **객체가 필요**하지만 **기능은 필요하지 않은 경우**에 사용
- 객체는 전달되지만 사용되지 않는 객체

```java
    public void print() {
        // 아무런 동작을 하지 않는다.
    }
```

## Fake

- 복잡한 로직이나 객체 내부에서 필요로 하는 다른 외부 객체들의 **동작을 단순화**하여 구현한 객체
- 동작의 구현을 가지고 있지만 실제 프로덕션에는 적합하지 않은 객체

```java
    public User findById(long id) {
        for (User user : users) {
            if (user.getId() == id) {
                return user;
            }
        }
        return null;
    }
```

## Stub

- Dummy 객체가 실제로 동작하는 것 처럼 보이게 만들어 놓은 객체
- 테스트에서 호출된 요청에 대해 **미리 준비해둔 결과**를 제공

```java
    public User findById(long id) {
        return new User(id, "Test User");
    }
```

## Spy

- 실제 객체로도 사용할 수 있고 Stub 객체로도 활용할 수 있음
- 필요한 경우  **호출된 내용**에 대해 약간의 **정보를 기록**해서 특정 메서드가 제대로 호출되었는지 여부를 확인할 수 있음

```java
    private int sendMailCount = 0;
    private Collection<Mail> mails = new ArrayList<>();

    public void sendMail(Mail mail) {
        sendMailCount++; //자기 자신이 호출된 상황 기록
        mails.add(mail);
    }
```

## Mock

- 호출에 대한 기대를 명세하고 내용에 따라 동작하도록 프로그래밍 된 객체





# 출처

- [Test Double을 알아보자](https://woowacourse.github.io/javable/post/2020-09-19-what-is-test-double/)
- [Test Double](http://xunitpatterns.com/Test%20Double.html)

