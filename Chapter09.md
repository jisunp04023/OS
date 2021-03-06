# Chapter 09 가상 메모리 관리

## 01 요구 페이징

* demand paging
  - 사용자가 요청할 때 해당 페이지를 메모리로 가져오는 정책
  - <-> prefetch: 반대로 앞으로 필요할 것으로 예상되는 페이지를 미리 가져오는 정책

* PTE
  - 페이지 번호, 플래그 비트, 프레임 번호로 구성

* PTE의 flag bit
  - access bit (접근 비트)
    - 페이지가 메모리에 올라온 후 사용한 적이 있는지
  - modified bit (변경 비트)
    - 페이지가 메모리에 올라온 후 데이터의 변경이 있었는지
  - valid bit (유효 비트)
  
    <img src = "https://user-images.githubusercontent.com/23165155/110591729-c42c3b80-81bc-11eb-84a2-c32112113619.png" width = "60%">
    
    - 페이지가 실제 메모리에 있는지
  - read, write, execute bit (읽기, 쓰기, 실행 비트)
    - 페이지에 대한 읽기, 쓰기, 실행 권한을 나타냄

* page fault (페이지 부재)
  - 프로세스가 페이지를 요청했을 때 그 페이지가 메모리에 없는 상황
  - 부재가 발생하면, 해당 페이지를 스왑 영역 -> 물리 메모리
  - 프로세스가 페이지 3을 요청

    <img src = "https://user-images.githubusercontent.com/23165155/110593857-7238e500-81bf-11eb-90be-fd221b099710.png" width = "65%">

    - 페이지 테이블의 유효비트를 보고 페이지 부재가 발생했음을 확인
    - 스왑 영역의 주소필드 0번에 있는 페이지를 메모리의 비어 있는 프레임 5로 가져옴 (swap in)
    - PTE3의 유효비트를 바꾸고, 주소필드도 프레임 번호 5로 업데이트
  - 프로세스가 페이지 4를 요청
    
    <img src = "https://user-images.githubusercontent.com/23165155/110593996-9d233900-81bf-11eb-94c5-fc8b30abffb8.png" width = "65%">

    - 페이지 테이블의 유효비트를 보고 페이지 부재가 발생했음을 확인
    - 스왑 영역의 주소필드 1번에 있는 페이지를 메모리로 가져오기 위해서는 victim page를 골라 스왑 영역으로 내보내야 함
    - 프레임 3에 저장된 페이지를 선정하고 스왑 영역으로 옮김 (swap out)
    - PTE0의 유효비트를 바꾸고, 프레임 번호 3도 주소필드 6으로 업데이트
    - 이제 스왑 영역의 주소필드 1번에 있는 페이지를 메모리의 비어 있는 프레임 3으로 가져옴 (swap in)
    - PTE4의 유효비트를 바꾸고, 주소필드도 프레임 번호 3으로 업데이트

* locality
  - 기억장치에 접근하는 패턴이 메모리 전체에 고루 분포되는 것이 아니라 특정 영역에 집중되는 성질
  - 페이지 교체 알고리즘이 쫓아낼 페이지를 찾을 때 지역성을 바탕으로 함
    - spatial locality (공간의 지역성): 현재 위치에서 가까운 데이터에 접근할 확률이 높다는 것
    - temporal locality (시간의 지역성): 현재를 기준으로 가장 가까운 시간에 접근한 데이터가 사용될 확률이 높다는 것
    - sequential locality (순차적 지역성): 작업이 순서대로 진해오디는 경향이 있다는 것

## 02 페이지 교체 알고리즘

* page replacement algorithm
  - 빈 프레임이 없을 때 어떤 페이지를 스왑 영역으로 내보낼지 결정하는 알고리즘

  <img src = "https://user-images.githubusercontent.com/23165155/110614493-bd5df280-81d5-11eb-8e2b-5e1e0dd05cdd.png" width = "%60">
  

* random page replacement algorithm (무작위)
  - 무작위로 대상 페이지를 선정하여 스왑 영역으로 보냄
  - 지역성 전혀 고려 X. 알고리즘의 성능 좋지 않음. 거의 사용 X

* FIFO(First In First Out) page replacement algorithm
  - 처음 메모리에 올라온 페이지를 스왑 영역으로 보냄
  - 큐로 구현하여 새로운 페이지는 항상 tail에 삽입, head의 페이지를 내보내고 이동시킴
  - 무조건 오래된 페이지를 선정하기 때문에 성능 떨어짐

* optimal page replacement algorithm (최적)
  - 미래의 접근 패턴을 보고 대상 페이지를 선정해 스왑 영역으로 보냄
  - 미래 접근 패턴 아는 것 불가능. 실제로 구현 불가

* LRU(Least Recently Used) page replacement algorithm
  - 시간적으로 멀리 떨어진 페이지를 스왑 영역으로 보냄
  - 페이지에 접근한 시각을 기록하거나 reference bit shift 이용해 구현 -> 메모리 낭비
  - 일반적으로 성능: FIFO < 2nd chance < LRU = LFU = NUR < optimal

* LFU(Least Frequently Used) page replacement algorithm
  - 사용 빈도가 적은 페이지를 스왑 영역으로 보냄
  - 접근 횟수를 표시하는 데 추가 공간이 필요 -> 메모리 낭비

* NUR(Not Used Recently) page replacement algorithm
  - 최근에 사용한 적이 없는 페이지를 스왑 영역으로 보냄
  - LRU, LFU와 성능 비슷. 공간 낭비 문제를 해결
  - 추가비트 2개만 사용
    - 참조 비트: 페이지에 접근(read/execute)하면 1
    - 변경 비트: 페이지 변경(write/append)되면 1

* FIFO 변형
  - FIFO 알고리즘을 변형하여 성능을 높임. 접근할 때마다 순서의 변화를 주어 성능을 향상
  - second chance page replacement algorithm
    - 페이지 부재 없이 성공할 경우 큐의 tail로 옮겨 기회를 한번 더 줌
    - 큐 유지 비용 높고, 큐 중간에 있는 값을 뒤로 이동하는 작업이 추가됨
  - clock algorithm
    - 2차 기회 페이지 교체 알고리즘 + 원형 큐
    - pointer가 옮길 대상 페이지를 가리킴
    - 2차 기회 알고리즘에 비해 참조비트가 하나씩 추가됨. 성공적으로 참조하면 참조비트 0->1
    - 참조 비트 1인 페이지 건너 뛸 때는 참조비트 1->0

      <img src = "https://user-images.githubusercontent.com/23165155/110613755-fa75b500-81d4-11eb-8f0c-555e41c8b86f.png" width = "75%">
      
    - pointer가 이동하는 경우
      - 요구 페이지가 없는 경우
      - 참조비트가 1인 경우
    - 참조비트 하나만 추가하기 때문에 NUR보다 추가 공간 적게 들지만, 알고리즘이 복잡하고 계산량 많음


## 03 스레싱 프레임 할당

* threshing
  - 하드디스크의 입출력이 너무 많아져서 잦은 페이지 부재로 작업이 멈춘 것 같은 상태
  - threshing point
    - 메모리가 차면 CPU가 작업하는 시간보다 스왑영역으로 페이지 보내고 새로운 페이지 가져오는 작업이 빈번해져 CPU가 작업할 수 없는 상태 되는 지점
  - threshing point를 늦추면 컴퓨터 빨라짐
  - 어떤 프로세스에 너무 적은 프레임 할당 -> 페이지 부재 빈번히 일어남
  - 어떤 프로세스에 너무 많은 프레임 할당 -> 메모리 낭비되어 전반적인 시스템 성능 떨어짐

* 프레임 할당 방식
  - static allocation (정적 할당): 프로세스 실행 초기에 프레임을 나누어준 후 그 크기를 고정하는 방식
    - equal allocation (균등 할당)
      - 프로세스의 크기와 상관 없이 사용 가능한 프레임을 모든 프로세스에 동일하게 할당
      - 큰 프로세스 -> 페이지 부재 빈번 / 작은 프로세스 -> 메모리 낭비
    - proportional allocation (비례 할당)
      - 프로세스의 크기에 비례하여 프레임을 할당
      - 프로세스 실행 중 필요로하는 프레임을 유동적으로 반영 X / 사용하지 않을 메모리를 미리 확보하여 공간 낭비

  - dynamic allocation (동적 할당): 프로세스를 실행하는 중에 프레임을 나누어주기도 하고 회수하기도 하는 방식
    - working set model (작업집합 모델)
      - 최근 일정 시간 동안 참조된 페이지 집합을 만듦. 이 집합의 페이지들을 물리 메모리에 유지
      - working set size: 작업집합에 들어갈 최대 페이지 수 (= 얼마나 자주 작업집합을 갱신할지)
      - WSW (Working Set Window): 작업집합에 포함되는 페이지의 범위
    - page fault frequency (페이지 부재 빈도)
      - 페이지 부재 횟수를 기록, 페이지 부재 비율을 계산
      - 상한선과 하한선을 설정. 상한선 초과 -> 프레임 추가 / 하한선 미만 -> 프레임 회수
