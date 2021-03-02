# Chapter 03 프로세스와 스레드

## 01 프로세스의 개요

* process: 실행을 위해 메모리에 올라온 동적인 상태
* program: 저장장치에 저장되어 있는 정적인 상태

* PCB (Process Control Block)
  - process = program + PCB
  - program = process - PCB
  
* process status (active status)

  <img src = "https://user-images.githubusercontent.com/23165155/109586610-de28a700-7b48-11eb-9710-f9e39556c252.PNG" width = "55%">

  + create status (생성 상태)
    - 프로그램을 메모리에 가져와 실행 준비가 완된 상태
    - 메모리 할당, PCB 생성
  
  + ready status (준비 상태)
    - 생성된 모든 프로세스가 CPU를 얻을 때까지 자기 차례를 기다리는 상태 (CPU scheduler가 프로세스를 선택)
    - dispatch(PID): 준비-> 실행
  
  + running status (실행 상태)
    - CPU를 얻어 실제 작업을 수행. 타임 슬라이스 동안 작업을 끝내지 못했다면 준비상태로 돌아와 다음 차례 기다림
    - timeout(PID): 실행-> 준비
    - exit(PID): 실행-> 완료
    - block(PID): 실행-> 대기
  
  + blocking status (대기 상태)
    - 실행 상태의 프로세스가 입출력을 요청하면 입출력이 완료될 때까지 기다리는 상태. 입출력 완료되면 준비 상태로 이동
    - wakeup(PID): 대기-> 준비
  
  +  terminate status (완료 상태)
    - 프로세스 종료된 상태. 사용한 데이터를 메모리에서 삭제, PCB 폐기 (정상 종료: exit, 비정상 종료: abort 포함)
    - 메모리 삭제, PCB 삭제

* pause status (휴식 상태)
  + 프로세스가 일시적으로 쉬고 있는 상태. 프로세스는 여전히 메모리에 있음
* suspend status (보류 상태)
  + 프로세스가 메모리에서 잠시 쫓겨난 상태. 프로세스는 swap area로 옮겨져 임시 보관됨

## 02 프로세스 제어 블록과 문맥 교환

* PCB
  + 프로세스를 실행하는 데 필요한 정보를 보관하는 자료구조
  + 모든 프로세스는 고유의 PCB를 가짐
  + 프로세스 생성 시 메모리의 OS 영역에 만들어져 실행 완료 시 폐기됨
  + 구성
    - 포인터, 프로세스 상태, 프로세스 구분자, 프로세스 카운터, 프로세스 우선순위
    - 각종 레지스터 정보, 메모리 관리 정보, 할당된 자원 정보, 계정 정보, PPID와 CPID

* context switching
  + CPU를 차지하던 프로세스가 나가고 새로운 프로세스를 받아들이는 작업
  + 두 프로세스의 PCB가 교환됨
  + 일반적으로 timeout 등 interrupt 시 발생

## 03 프로세스의 연산

* fork() 시스템 호출

  <img src = "https://user-images.githubusercontent.com/23165155/109609625-ee537d00-7b6e-11eb-8130-b3c3e0712383.PNG" width = "60%">
  
  + 실행 중인 프로세스로부터 새로운 프로세스를 복사하는 함수
  + 기존 프로세스는 부모 프로세스가 되고 새로 생긴 프로세스는 자식 프로세스가 됨 (부모-자식 관계)
  + 호출 후 프로세스의 변화
    - PID, PPID, CPID, 부모와 자식의 메모리 위치가 다르므로 메모리 관련 정보
  + 장점
    - 프로세스 생성 속도가 빠름
    - 추가 작업 없이 자원 상속 가능
    - 시스템 관리 효율적으로 가능: 자식이 종료되면 자원을 부모가 정리함
  + fork()
    - 부모라면 0 보다 큰 값 반환
    - 자식이라면 0 반환
    - 자식 생성되지 않았다면 0 보다 작은 값을 반환 (error)

* exec() 시스템 호출

  <img src = "https://user-images.githubusercontent.com/23165155/109611181-31aeeb00-7b71-11eb-93f6-67ead4dc34a0.PNG" width = "60%">
  
  + 이미 만들어진 프로세스의 구조를 그대로 둔 채 내용만 바꾸는 함수
  + PCB, 메모리 영역, 부모-자식 관계 그대로 사용
  + 호출 후 프로세스의 변화
    - 코드 영역, 데이터 영역, 스택 영역

* 프로세스의 계층 구조
  + 부모 프로세스를 복사해 자식 프로세스를 만들어 프로세스끼리 계층 구조를 갖는 것
  + 장점
    - 여러 작업의 동시 처리
    - 용이한 자원 회수: 자식이 작업 마치면 사용하던 자원을 부모가 회수
   
   + orphan process (= zombie process)
    - 부모가 먼저 종료되거나, 자 비정상적 종료되어 부모와 연락이 안 되는 경우 비정상적으로 남아 있는 프로세스
    - 방지하기 위해-> 부모: wait() / 자식: exit(), return()

## 04 스레드

* thread
