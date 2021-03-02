# Chapter 03 프로세스와 스레드

## 01 프로세스의 개요

* process: 실행을 위해 메모리에 올라온 동적인 상태
* program: 저장장치에 저장되어 있는 정적인 상태

* PCB (Process Control Block)
  - process = program + PCB
  - program = process - PCB
  
* process status (active status)
  + create status (생성 상태)
    - 프로그램을 메모리에 가져와 실행 준비가 완려된 상태
    - 메모리 할당, PCB 생성
  + ready status (준비 상태)
    - 생성된 모든 프로세스가 CPU를 얻을 때까지 자기 차례를 기다리는 상태 (CPU 스케줄러가 프로세스를 선택)
    - dispatch(PID): 준비-> 실행
  + running status (실행 상태)
    - CPU를 얻어 실제 작업을 수행. 타임 슬라이스 동안 작업을 끝내지 못했다면 준비상태로 돌아와 다음 차례 기다림
    - timeout(PID): 실행-> 준비
    - exit(PID): 실행-> 완료
    - block(PID): 실행-> 대기
  + blocking status (대기 상태)
    - 실행 상태의 프로세스가 입출력을 요청하면 입출력이 완료될 때까지 기다리는 상태. 입출력 완료되면 준비 상태로 이동
    - wakeup(PID): 대기-> 준비
  + terminate status (완료 상태)
    - 프로세스 종료된 상태. 사용한 데이터를 메모리에서 삭제, PCB 폐기 (정상 종료: exit, 비정상 종료: abort 포함)
    - 메모리 삭제, PCB 삭제

* pause status (휴식 상태)
* suspend status (


## 02 프로세스 제어 블록과 문맥 교환


## 03 프로세스의 연산


## 04 스레드
