# Deep Sleep 관련 코드 설명 (MicroPython)

## 주요 코드 및 설명

### 1. `rtc = machine.RTC()`
- RTC(Real-Time Clock) 객체 생성
- 슬립 모드에서도 작동 가능
- RTC를 이용해 일정 시간 후 자동으로 기기를 깨울 수 있음


### 2. `rtc.irq(trigger=rtc.ALARM0, wake=machine.DEEPSLEEP)`
- RTC 인터럽트 설정
- `rtc.ALARM0` 발생 시 슬립 모드 해제
- `wake=machine.DEEPSLEEP` 으로 설정 시 Deep Sleep에서 자동으로 복귀 가능


### 3. `rtc.alarm(rtc.ALARM0, duration_ms)`
- RTC 알람 타이머 설정
- `duration_ms` 밀리초(ms) 후 알람 발생
- 슬립 시간 지정 예시:
  - 10초: `rtc.alarm(rtc.ALARM0, 10000)`
  - 60초: `rtc.alarm(rtc.ALARM0, 60000)`
  - 10분: `rtc.alarm(rtc.ALARM0, 600000)`


### 4. `machine.deepsleep()`
- Deep Sleep 모드로 진입
- 대부분의 하드웨어가 꺼지고 초저전력 상태가 됨
- 지정된 RTC 알람 시간 이후 자동으로 재시작됨
- 재시작 시 `boot.py` → `main.py` 순서로 실행됨


### 5. `machine.reset_cause()`
- 리셋(재시작)의 원인을 확인
- 슬립 해제 후 기기가 부팅된 경우, 다음 중 하나를 반환:
  - `machine.DEEPSLEEP_RESET`: Deep Sleep에서 깨어남
  - `machine.PWRON_RESET`: 일반 전원 부팅


## 코드 흐름 예시 (공통 구조)

```text
1. RTC 객체 생성
2. 알람 인터럽트 및 wake 조건 설정
3. 슬립 시간(ms) 설정
4. Deep Sleep 진입
5. 설정된 시간이 지나면 자동으로 깨어나 재시작
