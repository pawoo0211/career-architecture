# career-architecture
- 이름: 박상권

## 🚀미션
- 결제 알람(Notification) 기능 개선

### 개선포인트 분석
- 결제가 승인되거나 취소가 될 경우 가맹점 측으로 알림을 전송한다.
- 가맹점측에서 알람에 대한 처리 트래픽이 많거나 가맹점 측 서버가 다운 되어 알람을 못받을 경우 알람을 재전송한다.
- 재전송 알람을 못받을 경우 해당 쓰레드가 sleep을 하여 5초, 15초, 30초, 1분씩 시간 간격을 늘려 재전송한다.
- 결제 기능과 알람 기능이 하나의 서버에 존재하기에 위 상황의 경우 서버의 쓰레드를 계속 가지고 있게 되어 다른 기능에도 영향을 준다.
- 최종적으로 1분 동안 가맹점 측이 알람을 받지 못할 경우 포스트맨을 이용하여 수동으로 알람을 전송한다.

### 프로세스
flowchart TB
    A[Start] -- 주문 요청 -> B(주문 승인)
    B --> C[알람 전송]
    C -- Success --> D[주문완료]
    C -- Fail --> E[알람 재전송]
    E -- Max retry fail --> F[수동 알람 재전송]
