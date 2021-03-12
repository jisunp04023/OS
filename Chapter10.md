# Chapter 10 입출력 시스템과 저장장치

## 01 입출력 시스템

* 주변 장치 구분
  - 저속 주변장치: 메모리와 주변장치 사이 오가는 데이터 양이 적어 데이터 전송률이 낮은 장치 (ex. 키보드)
  - 고속 주변장치: 메모리와 주변장치 사이 대용량 데이터가 오가므로 데이터 전송률이 높은 장치 (ex. 그래픽카드, 하드디스크)

* 입출력 버스의 구조
  - 메인버스: CPU와 메모리를 연결
  - 그래픽 버스: CPU와 그래픽카드를 연결
  - 고속 입출력 버스, 저속 입출력장치 버스

* DMA (직접 메모리 접근)
  - CPU 도움 없이도 메모리에 접근할 수 있도록 입출력 제어기에 부여된 권한
  - 입출력 제어기는 여러 주변장치로부터 전송된 데이터를 적절히 배분하여 어떤 것을 메모리로 보낼지 결정

* 인터럽트
  - 외부 인터럽트: 입출력 및 하드웨어 관련 인터럽트 (주변장치 변화, 하드웨어 이상)
  - 내부 인터럽트: 프로세스의 오류로 발생하는 인터럽트 (예외 상황 인터럽트)
  - 시그널: 사용자의 요청으로 발생하는 인터럽트 (자발적 인터럽트)

* 인터럽트 벡터
  - 여러 인터럽트 중 어떤 인터럽트가 발생했는지 파악하기 위한 자료구조

* 인터럽트 핸들러
  - 인터럽트 처리 방법을 함수 형태로 만들어놓은 것

## 02 디스크 장치

* HDD (Hard Disk Drive)
  
  <img src = "https://user-images.githubusercontent.com/23165155/110874804-c22abf80-8317-11eb-809a-bf2e580d2dd7.png" width = "50%">
  
  - 원반을 사용한 저장장치. 맨 앞 데이터나 맨 뒤 데이터나 접근하는 속도가 거의 비슷
  - platter: 표면에 자성체가 발려 자기를 이용해 0, 1을 저장
  - sector, block: 섹터는 하드웨어 입장에서 하드디스크의 가장 작은 저장 단위 (블록은 OS 입장에서 가장 작은 저장 단위)
  - track, cylinder: 트랙은 동일한 동심원상에 있는 섹터의 집합. 실린더는 여러개의 플래터에 있는 같은 트랙의 집합
  - head: 데이터를 읽거나 쓸 때는 read/write head를 사용

* 하드디스크와 CD 비교
  - HD
    - constant angular velocity (각속도 일정 방식의 회전)
      - 바깥쪽 트랙의 속도가 안쪽보다 빠름. 바깥쪽 섹터가 안쪽 트랙보다 더 큼. 모든 트랙의 섹터 수 같음
  - CD
    - constant linear velocity (선속도 일정 방식의 회전)
      - 모든 트랙의 움직이는 속도 같음, 섹터 크기도 같음. 바깥쪽 트랙에 섹터 수 더 많음

* 디스크 장치의 전송 시간
  - 하드디스크에서 데이터 가져오는 데 걸리는 총 시간 (= 탐색 시간 + 회전 지연 시간 + 전송 시간)
    - seek time: 현재 위치부터 섹터가 있는 트랙까지 헤드가 이동하는데 걸리는 시간
    - rotational latency time: 원하는 섹터를 만날때까지 플래터가 회전하는 시간
    - transmission time: 데이터를 읽어 전송하는데 걸리는 시간
  - 성능을 높이려면 가장 많은 비중 차지하는 탐색시간을 최소화 해야 함 -> 조각 모음, 디스크 스케줄링 기법 사용

* 디스크 장치 관리
  - partition: 디스크를 논리적으로 분할하는 작업
  - formating: 디스크 표면을 초기화하는 작업
  - defragmentation: 디스크에 파일을 저장했다 지우기를 반복하여 중간중간 생긴 빈 공간을 하나로 모으는 작업

* 네트워크 저장장치
  - DAS (Direct Attached Storage)
    
    <img src = "https://user-images.githubusercontent.com/23165155/110875076-4f6e1400-8318-11eb-9b8d-f54ef9202baf.png" width = "40%">
    
    - 서버와 같은 컴퓨터에 직접 연결된 저장장치
  - NAS (Network Attached Storage)
    
    <img src = "https://user-images.githubusercontent.com/23165155/110875192-83e1d000-8318-11eb-9473-0505872631de.png" width = "40%">
    
    - 기존의 저장장치를 LAN이나 WAN에 붙어서 사용하는 방식
  - SAN (Storage Area Network)
    
    <img src = "https://user-images.githubusercontent.com/23165155/110875298-b390d800-8318-11eb-9126-b250995e62b5.png" width = "50%">
    
    - 데이터 서버, 백업 서버, RAID 등의 장치를 네트워크로 묶고 데이터 접근을 위한 서버를 두는 형태


## 03 디스크 스케줄링

  <img src = "https://user-images.githubusercontent.com/23165155/110885869-f52a7e80-832a-11eb-99c0-87cfee7aedc0.png" width = "45%">


* FCFS(First Come, First Service) disk scheduling
  
  <img src = "https://user-images.githubusercontent.com/23165155/110885929-125f4d00-832b-11eb-9683-8e68e9239259.png" width = "55%">
  
  - 가장 단순. 트랙 요청이 들어온 순서대로 서비스

* SSTF(Shortest Seek Time First) disk scheduling
  
  <img src = "https://user-images.githubusercontent.com/23165155/110885990-33c03900-832b-11eb-90e0-d0dbfdc79c84.png" width = "55%">
  
  - 현재 헤드가 있는 위치에서 가장 가까운 트랙부터 서비스. 같다면 먼저 요청한 트랙을 서비스
  - 효율성 좋지만 아사 현상 -> 공평성 위배

* block SSTF disk scheduling
  
  <img src = "https://user-images.githubusercontent.com/23165155/110886063-53576180-832b-11eb-91b7-e9d8e2b8f543.png" width = "50%">
  
  <img src = "https://user-images.githubusercontent.com/23165155/110886117-77b33e00-832b-11eb-8f42-d81f2d891d35.png" width = "55%">
  
  - SSTF의 공평성 위배를 어느정도 해결. 에이징을 적용
  - 큐에 있는 트랙 요청을 일정한 블록 형태로 묶음
  - 현재 트랙에서 가장 먼 트랙을 블록의 끝으로 이동 (aging) -> 공평성 보장
  - 성능: FCFS = block SSTF < SCAN < SSTF

* SCAN disk scheduling
  
  <img src = "https://user-images.githubusercontent.com/23165155/110886188-9a455700-832b-11eb-966d-6d095c01ef8d.png" width = "55%">
  
  - SSTF의 공평성 위배를 완화. 헤드가 움직이기 시작하면 맨 마지막 트랙에 도착할 때까지 뒤돌아가지 않고 전진하며 서비스
  - 동일한 트랙/실린더 요청이 연속적으로 발생하면 헤드가 제자리에 머물러 바깥쪽 트랙이 아사현상

* C-SCAN (Circular SCAN) disk scheduling
  
  <img src = "https://user-images.githubusercontent.com/23165155/110886266-b943e900-832b-11eb-8214-320daece8f33.png" width = "55%">
  
  - 헤드가 한쪽 방향으로 움직일 때는 요청받은 트랙을 서비스하지만 반대로 돌아올 때는 서비스하지 않고 헤드만 이동
  - 모든 트랙의 방문 횟수가 동일하지만. / 작업 없이 헤드 이동 -> 비효율적
  - 동일한 트랙/실린더 요청이 연속적으로 발생하면 헤드가 제자리에 머물러 바깥쪽 트랙이 아사현상
  - 성능: C-SCAN < SCAN < LOOK

* LOOK disk scheduling
  
  <img src = "https://user-images.githubusercontent.com/23165155/110886337-d2e53080-832b-11eb-81f2-6fd5b23df7b9.png" width = "55%">
  
  - C-SCAN과 유사하지만 더 이상 서비스할 트랙이 없으면 헤드가 끝까지 가지 않고 중간에 방향을 바꿈

* C-LOOK (Circular LOOK) disk scheduling
  
  <img src = "https://user-images.githubusercontent.com/23165155/110886437-f4deb300-832b-11eb-922c-e4113776e376.png" width = "55%">
  
  - C-SCAN의 LOOK 버전. 더 이상 서비스할 트랙이 없으면 헤드가 중간에 방향을 바꿈

* SLTF(Shortest Latency Time First) disk scheduling
  
  <img src = "https://user-images.githubusercontent.com/23165155/110886508-163f9f00-832c-11eb-8ce9-8228df0971af.png" width = "35%">
  
  - 헤드가 고정된 저장장치에서 사용. 요청이 들어온 섹터의 순서를 디스크가 회전하는 방향에 맞추어 다시 정렬한 후 서비스
  - 탐색 시간이 없어서 매우 빠르게 데이터 주고 받지만, 매우 고가

## 04 RAID

* RAID
  - 자동으로 백업을 하고 장애가 발생하면 이를 복구하는 시스템
  - 동일한 규격의 디스크를 여러 개 모아 구성하여 장애가 발생했을 때 데이터를 복구하는 데 사용
  - 0 1 2 3 4 5 6 0+1 10 50 60
