# Thread.sleep()과 Task.sleep()의 차이

| **`Thread.sleep()`**               | **`Task.sleep()`**                                     |
| ---------------------------------- | ------------------------------------------------------ |
| 스레드 block                       | suspend 및 다른 task 실행 허용                         |
| 동일한 코어에서 스레드의 느린 전환 | function calls to a continuation 을 통한 빠른 전환     |
| 취소 불가                          | Suspended 상태에서 취소 가능                           |
| 코드는 동일한 스레드에서 재개      | 다른 스레드에서 재개할 수 있음. 스레드는 중요하지 않음 |



# 출처

- [The difference between Thread.sleep() and Task.sleep()](https://trycombine.com/posts/thread-task-sleep/)

