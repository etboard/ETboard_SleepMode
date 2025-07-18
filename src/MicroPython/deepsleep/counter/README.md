# ESP32 Deep Sleep 관련 코드 설명 (MicroPython)

## 주요 코드 및 설명

### 1. `rtc.irq(trigger=rtc.ALARM0, wake=machine.DEEPSLEEP)`
- RTC 인터럽트 설정  
- `rtc.ALARM0` 발생 시 슬립 모드 해제  
- `wake=machine.DEEPSLEEP` 으로 설정 시 Deep Sleep에서 자동으로 복귀 가능  


### 2. `rtc.alarm(rtc.ALARM0, duration_ms)`
- RTC 알람 타이머 설정  
- `duration_ms` 밀리초(ms) 후 알람 발생  
- 슬립 시간 지정 예시:  
  - 10초: `rtc.alarm(rtc.ALARM0, 10000)`  
  - 60초: `rtc.alarm(rtc.ALARM0, 60000)`  
  - 10분: `rtc.alarm(rtc.ALARM0, 600000)`  


### 3. `machine.deepsleep()`
- Deep Sleep 모드로 진입  
- 대부분의 하드웨어가 꺼지고 초저전력 상태가 됨  
- 지정된 RTC 알람 시간 이후 자동으로 재시작됨  
- 재시작 시 `boot.py` → `main.py` 순서로 실행됨  


### 4. `machine.reset_cause()`
- 리셋(재시작)의 원인을 확인  
- 슬립 해제 후 기기가 부팅된 경우, 다음 중 하나를 반환:  
  - `machine.DEEPSLEEP_RESET`: Deep Sleep에서 깨어남  
  - `machine.PWRON_RESET`: 일반 전원 부팅  


### 5. 카운터 유지 방식 (NVS 저장소 활용)
- `machine.nvs_getint("group", "key")` 및 `machine.nvs_setint(...)` 사용
- 비휘발성 저장소(NVS)에 값 저장 → 재부팅 이후에도 값 유지 가능
- 예제에서는 부팅 횟수 또는 실행 카운터를 저장하여 상태 추적


## 코드 흐름 예시 (공통 구조)

```text
1. NVS에서 카운트 값 불러오기
2. 현재 카운트를 출력
3. 카운트 +1 후 NVS에 저장
4. RTC 슬립 타이머 설정
5. Deep Sleep 진입
