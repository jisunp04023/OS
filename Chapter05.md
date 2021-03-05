# Chapter 05 프로세스 동기화

## 01 프로세스 간 통신

* 프로세스 간 통신 종류
  - 프로세스 내부 데이터 통신
    - 하나의 프로세스 내 2개 이상의 스레드가 전역변수나 파일 이용해 데이터를 통신
  - 프로세스 간 데이터 통신
    - 같은 컴퓨터 내 여러 프로세스끼리 공용 파일이나 OS가 제공하는 파이프를 통해 통신
  - 네트워크를 이용한 데이터 통신
    - 여러 컴퓨터가 네트워크로 연결되어 있을 때 소켓을 이용해 통신 (= 네트워킹)

* 통신 방향에 따른 분류
  - duplex communication (양방향 통신)
    - 동시에 양쪽 방향으로 전송 가능 (일반적인 통신 소켓)
  - half-duplex communication (반양방향 통신)
    - 양쪽 방향으로 전송 가능하지만 동시 전송은 불가 (무전기)
  - simplex communication (단방향 통신)
    - 한쪽 방향으로만 전송 가능 (전역 변수, 파일, 파이프)

* 통신 구현 방식에 따른 분류
  - blocking communication (= synchronous communication)
    - 대기가 있는 통신. 동기화 통신 (파이프, 소켓)
    - 데이터가 도착할 때까지 자동으로 대기 상태에 머무름. 운영체제가 알아서 알려줌
  - non-blocking communication (= asynchronous communication)
    - 대기가 없는 통신. 비동기화 통신 (전역 변수, 파일)
    - busy waiting을 사용해 사용자가 직접 데이터 전송 여부 확인

* 프로세스 간 통신의 종류

  <img src = "https://user-images.githubusercontent.com/23165155/110062953-96ef2000-7dad-11eb-8893-590ac4061b9f.png" width = "50%">
  
  - 전역 변수를 이용한 통신
    - 공동으로 관리하는 메모리를 사용해 데이터를 주고 받는 것
    - 직접적으로 관련 있는 프로세스 간 사용. (ex. 부모-자식 프로세스 간) extern 사용하면 관련 없는 프로세스 간도 가능
  - 파일을 이용한 통신
    - 파일을 open, write, read, close (fd, file descriptor 사용)
  - 파이프를 이용한 통신
    - OS가 제공하는 동기화 방식. 바쁜 대기 X
    - open() 통해 fd 얻어 작업 후 close(). 단방향 통신 (양방향 통신 위해선 파이프 2개 사용)
    - annonymous pipe- 서로 관련 있는 프로세스 간 / named pipe- 서로 관련 없는 프로세스 간
  - 소켓을 이용한 통신
    - 다른 컴퓨터에 있는 함수를 호출
    - binding: 소켓을 매개로 한쪽 프로세스와 반대쪽 프로세스를 연결하는 작업
    - 동기화 지원. 바쁜 대기 X
    - 소켓 하나 만으로 양방향 통신 가능

## 02 공유 자원과 임계구역

* shared resource
  - 여러 프로세스가 공동으로 이용하는 변수, 메모리, 파일 등
  - race condition: 2개 이상의 프로세스가 공유 자원을 병행적으로 읽거나 쓰는 상황

* critical section (임계구역)
  - 공유 자원 접근 순서에 따라 실행 결과가 달라지는 프로그램의 영역. 공유할 수 없는 중요한 자원

* 임계구역 해결 조건
  - mutual exclusion (상호 배제)
    - 한 프로세스가 임계구역에 들어가면 다른 프로세스는 들어갈 수 없음
  - bounded waiting (한정 대기)
    - 어떤 프로세스도 infinite postpone(무한 대기)하지 않아야 함. 특정 프로세스가 임계구역이 진입하지 못하면 안 됨
  - progress flexibility (진행의 융통성)
    - 한 프로세스가 다른 프로세스의 진행을 방해해서는 안 됨

## 03 임계구역 해결

* 상호 배제 문제

  <img src = "https://user-images.githubusercontent.com/23165155/110069142-8c875300-7dba-11eb-980e-8d04637c4aa3.png" width = "50%">
  
  - P1이 1번을 실행. 잠금이 걸려 있지 않으니 루프를 빠져나옴. 타임아웃
  - P2가 2번을 실행. 아직도 잠금이 걸려 있지 않으니 루프문 빠져나옴. 타임아웃
  - P1은 3번에서 잠금을 걸고 임계구역에 진입. 타임아웃
  - P2도 4번에서 잠금을 걸고 임계구역에 진입.
    - 둘 다 임계구역에 진입 -> 상호 배제 조건 보장 X
    - 또 잠금 풀리기를 기다리며 바쁜대기(루프문)를 해야 함 -> 시스템 자원 낭비

* 한정 대기 문제

  <img src = "https://user-images.githubusercontent.com/23165155/110069316-e2f49180-7dba-11eb-9196-106b9f7b5b85.png" width = "50%">
  
  - P1은 1번에서 lock1의 잠금을 걸고 타임아웃
  - P2가 2번에서 lock2의 잠금을 걸고 타임아웃
  - P1은 lock2가 잠금 걸린 상태라서 3번에서 무한 루프에 빠짐. 타임아웃
  - P2도 lock1이 잠금 결린 상태라서 4번에서 무한 루프에 빠짐
    - 두 프로세스 모두 임계구역이 진입하지 못함 -> 무한 대기. 즉 한정 대기 조건 보장 X (= deadlock, 교착상태)
    - 확장성 문제: 프로세스 늘수록 lock의 개수도 늘어남 -> 비효율적

* 진행의 융통성 문제

  <img src = "https://user-images.githubusercontent.com/23165155/110069376-01f32380-7dbb-11eb-896e-1fcf74340eb1.png" width = "50%">
  
  - P1부터 서로 번갈아가며 임계구역이ㅔ 진입
    - lockstep synchronization(경직된 동기화): 프로세스 진행이 다른 프로세스로 인해 방해받는 현상 -> 진행의융통성 보장 X

* 하드웨어적인 해결 방법

  <img src = "https://user-images.githubusercontent.com/23165155/110069450-28b15a00-7dbb-11eb-8234-208bd6733880.png" width = "50%">
  
  - test-and-set 코드로 하드웨어 지원 받아 한꺼번에 실행
    - 바쁜 대기를 사용 -> 자원 낭비

* Peterson algorithm

  <img src = "https://user-images.githubusercontent.com/23165155/110069142-8c875300-7dba-11eb-980e-8d04637c4aa3.png" width = "50%">
  
  - 두 프로세스가 동시에 lock을 잠갔더라도 turn을 사용해 양보함
    - 2개의 프로세스만 사용하다는 한계
* Dekker algorithm

  <img src = "https://user-images.githubusercontent.com/23165155/110069142-8c875300-7dba-11eb-980e-8d04637c4aa3.png" width = "50%">
  
  - P1이 lock1에 잠금을 걸고 타임아웃
  - P2도 lock2에 잠금을 걸고 타임아웃
  - P1은 lock2에 잠금이 걸렸는지 확인
  - 만약 걸렸다면, 누가 먼저인지 turn을 확인
  - 만약 P1의 차례(turn=1)라면 임계구역으로 진입
  - 만약 P2의 차례(turn=2)라면 lock1의 잠금을 풀고 P1의 차례가 오길 기다림.
  - P1의 차례가 오면(turn=1) lock1에 잠금을 걸고 임계구역으로 진입
    - 피터슨/데커 알고리즘 모두 임계구역 세 가지 조건 만족하지만 매우 복잡

* semaphore
  - 임계구역에 진입하기 전에 스위치를 사용중으로 놓고 들어갔다가 나올 때 비었음을 표시
  - P(): 사용중인 상태 / V(): 비어있는 상태
    - 바쁜 대기를 하거나 동기화 메시지 보낼 필요 X
    - 

* monitor
  - 세마포어 알고리즘을 자동으로 처리하도록 설계한 코드. 시스템 호출과 같은 개념
  - 보호할 자원을 임계구역에 숨기고 공유 자원에 접근하기 위한 인터페이스만 제공하여 자원을 보호
  - wait(): semaphore의 P() / signal(): semaphore의 V()
    - 사용자 입장: 복잡한 코드 실행하지 않아서 좋음
    - 시스템 입장: 임계구역을 보호할 수 있어서 좋음
