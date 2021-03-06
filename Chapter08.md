# Chapter 08 가상 메모리의 기초

## 01 가상 메모리의 개요

* virtual memory
  - 물리 메모리의 크기와 상관없이 프로세스에 커다란 메모리 공간을 제공하는 기술
  - 프로세스는 OS가 어디에 있는지 물리 메모리 크기가 어느 정도인지 신경 쓰지 않고 메모리를 사용할 수 있음
  - virtual memory = main memory + swap area
  - DAT (Dynamic Address Translation) (동적 주소 변환): 가상 주소를 실제 메모리의 물리 주소로 변환하는 작업

    <img src = "https://user-images.githubusercontent.com/23165155/110404596-7b905780-80c2-11eb-98d8-d046925b4858.png" width = "55%">


* mapping table
  - 가상 주소가 물리 메모리의 어느 위치에 있는지 알 수 있도록 정리한 표
  - page mapping table, segmentation mapping table

## 02 페이징 기법

* 페이징 기법
  - 고정 분할 방식 이용한 가상 메모리 관리 기법
  - 물리 주소 공간을 같은 크기로 나누어 사용
  - page: 가상 주소에서 분할된 각 영역 / frame: 물리 메모리의 각 영역

* 주소 변환: VA = <P, D> -> PA = <F, D>
  
  <img src = "https://user-images.githubusercontent.com/23165155/110407124-ae3c4f00-80c6-11eb-864d-6271c04580d8.png" width = "75%">
  
  - 프로세스가 가상 주소 30번지 내용을 read: VA = <3, 0> -> PA = <1, 0>
    - 가상 주소 30번지가 어느 페이지에 있는지 찾음. 페이지3의 0번째 위치
    - 페이지 테이블을 보고 페이지3이 프레임1에 있음을 확인
    - 물리 주소 프레임1의 0번째 위치에 접근하여 읽음
  - 프로세스가 가상 주소 18번지에 값을 write: VA = <1, 8> -> PA = <3, 8>
    - 가상 주소 18번지가 어느 페이지에 있는지 찾음. 페이지1의 8번째 위치
    - 페이지 테이블을 보고 페이지1이 프레임3에 있음을 확인
    - 물리 주소 프레임3의 8번째 위치에 값을 저장

  - invalid는 해당 페이지가 swap area에 있다는 의미
  - PTE(Page Table Entry): page table의 각 row
  - PTBR(Page Table Base Register): 메모리 관리자가 빠르게 접근하기 위해 page table의 시작 주소를 보관하는 레지스터
  - 페이지 테이블의 크기가 너무 커지면 물리 메모리 영역이 줄어듦. 페이지 테이블의 일부도 swap area로 옮길 수 있음

* 페이지 테이블 매핑 방식

  <img src = "https://user-images.githubusercontent.com/23165155/110557979-fa4dc900-8184-11eb-89b7-d93cb8bbaa17.png" width = "75%">
  
  - direct mapping (직접 매핑)
    
    <img src = "https://user-images.githubusercontent.com/23165155/110586238-39940e00-81b5-11eb-919e-676f761cd668.png" width = "55%">
    
    - 페이지 테이블 전체가 물리 메모리의 운영체제 영역에 존재하는 방식
    - 별다른 부가 작업 없이 바로 주소 변환이 가능

  - associative mapping (연관 매핑)
    
    <img src = "https://user-images.githubusercontent.com/23165155/110586391-8677e480-81b5-11eb-9981-785d8e6ecde7.png" width = "60%">
    
    - 페이지 테이블 전체를 스왑 영역에서 관리하는 방식. 일부만 무작위로 물리 메모리로 가져옴
    - TLB(Translation Look-aside Buffer)(변환 색인 버퍼): 그 일부분의 테이블
      - TLB hit: 원하는 페이지 번호가 TLB에 있는 경우
      - TLB miss: 원하는 페이지 번호가 TLB에 없는 경우
    - 물리 메모리 공간이 작을 때 사용
    - 장점: 메모리를 절약할 수 있음
    - 단점: TLB miss 빈번하다면 -> 성능 떨어짐 / 물리 메모리 내 페이지 테이블 모두 검색, 없다면 스왑 영역 검색 -> 시간 낭비

  - set-associative mapping (직접-연관 매핑)
    
    <img src = "https://user-images.githubusercontent.com/23165155/110586515-b4f5bf80-81b5-11eb-9ecc-7218c60dc638.png" width = "60%">
    
    - 연관 매핑의 문제를 개선. 페이지 테이블을 일정한 집합으로 자르고, 자른 덩어리 단위로 물리 메모리에 가져옴
    - directory table에 일정하게 자른 페이지 테이블이 물리 메모리에 있는지 스왑 영역에 있는지 위치 정보 표시함
    - 원하는 페이지 테이블 엔트리가 어디있는지 간단히 파악 가능. 모든 페이지 테이블 검사할 필요 X -> 시간 단축

  - invert mapping (역매핑)
    
    <img src = "https://user-images.githubusercontent.com/23165155/110586651-de165000-81b5-11eb-9277-2f985ac1dcdc.png" width = "40%">
    <img src = "https://user-images.githubusercontent.com/23165155/110586773-0900a400-81b6-11eb-9fdc-27f83b6ffcfa.png" width = "30%">
    
    - 물리 메모리의 프레임 번호를 기준으로 테이블을 구성
    - 위 세 가지 매핑 방식은 테이블 크기가 프로세스 수에 비례했지만, 역매핑 방식에서는 크기가 일정하게 유지됨
    - 장점: 테이블의 크기가 매우 작음
    - 단점
      - 프로세스가 가상 메모리에 접근할 때 PID와 페이지 번호를 모두 찾아야 함. 모든 페이지 검색 -> 속도 느림
      - 페이지 테이블을 다 검사한 후에야 저장장치 스왑 영역에 접근 -> 시간 낭비


## 03 세그먼테이션 기법

* 세그먼테이션 기법
  - 가변 분할 방식 이용한 가상 메모리 관리 기법
  - 물리 메모리를 프로세스의 크기에 따라 가변적으로 나누어 사용
  - 장점: 메모리를 프로세스 단위로 관리하기 때문에 페이지 테이블이 작고 단순
  - 단점: 물리 메모의 외부 단편화로 인해 물리 메모리 관리가 복잡
  - limit: 세그먼트의 크기를 나타냄 / address: 물리 메모리상의 시작주소를 나타냄

* 주소 변환: VA = <S, D> -> PA
  
  <img src = "https://user-images.githubusercontent.com/23165155/110582860-26cb0a80-81b0-11eb-97b3-61e34991b736.png" width = "65%">
  
  - 프로세스 A의 32번지에 접근: VA = <0, 32>
    - 프로세스 A는 세그먼트 0으로 분할돼었으므로 S는 0, D는 32
    - 세그먼테이션 테이블에서 세그먼트0의 limit을 확인. 만약, D >= limit 이라면 trap(주소 오류)
    - 세그먼테이션 테이블에서 세그먼트0의 시작주소(address)를 알아낸 후 distance를 더함 (120 + 32 = 152)
    - 물리주소 152번지임

## 04 세그먼테이션-페이징 혼용 기법

* 세그먼테이션-페이징 혼용 기법
  - 사용자 입장에서는 세그먼테이션 기법, 메모리 관리자 입장에서는 페이징 기법
  - 서로 관련 있는 영역을 하나의 세그먼트로 묶어 세그먼테이션 테이블로 관리
  - 각 세그먼트 구성하는 페이지를 해당 페이지 테이블로 관리
  - 각 세그먼테이션 테이블은 자신과 연결된 페이지 테이블의 시작 주소를 가짐
  - 메모리 보호, 중복 보호를 세그먼테이션 테이블에서 관리함으로써 메모리 관리를 효율적으로 가능

* 주소 변환: VA = <S, P, D>
  
  <img src = "https://user-images.githubusercontent.com/23165155/110585308-f71e0180-81b3-11eb-9b7e-1f9fd91d22dc.png" width = "75%">
  
  - 사용자가 요청한 데이터의 주소가 몇 번째 세그먼트의 몇 번째 페이지로부터 얼마나 떨어져 있는지 계산 (VA = <S, P, D>)
  - 세그먼테이션 테이블을 보고 trap 발생시켜야 하는지 확인
  - 페이지 테이블을 보고 해당 페이지가 어떤 프레임에 저장되었는지 찾음
  - 물리 메모리라면 바로 접근, 없다면 스왑 영역에 가서 해당 페이지를 물리 메모리로 가져옴
  - 물리 메모리에 있는 프레임의 처음 위치에서 D만큼 떨어진 곳에 접근
