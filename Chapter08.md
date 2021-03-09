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
  - 가상 주소에서 분할된 각 영역: page / 물리 메모리의 각 영역: frame

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

* 페이지 테이블 매핑 방식
  - 직접 매핑
  - 연관 매핑
  - 직접-연관 매핑
  - 역매핑


## 03 세그먼테이션 기법


## 04 세그먼테이션-페이징 혼용 기법
