# Test Double

- 테스트를 진행하기 어려운 경우 이를 대신해 테스트를 진행할 수 있도록 만들어주는 객체

  ![test double](https://woowacourse.github.io/javable/static/a220a43d06adaa707959533541c8fbe7/40619/2020-09-19-test-double.jpg)

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

