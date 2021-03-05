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


## 03 임계구역 해결
