# Chapter 07 물리 메모리 관리

## 01 메모리 관리의 개요

* MMS(Memory Management System)
  - 여러 작업을 동시에 처리하는 복잡한 메모리 관리를 담당

* 메모리 관리의 이중성: 두 입장의 충돌
  - 프로세스 입장: 메모리 독차지하고자 함. 작업의 편리함을 요구
  - 메모리 관리자 입장: 되도록 효율적으로 관리하고자 함. 관리의 편리함 요구

* compiler
  - 소스코드를 기계어로 번역(컴파일)한 후 한꺼번에 실행 (C, Java 등)
  - 변수가 미리 선언되어야 함
  - 장점: 오류 찾기와 코드 최적화, 분할 컴파일에 의한 공동 작업 가능
  - 사용 프로그램: 대형 프로그램
  - 과정
    - 소스코드 작성 및 컴파일: object code가 만들어짐
    - 목적코드와 라이브러리 연결: 라이브러리에 있는 코드를 목적 코드에 삽입
    - 동적 라이브러리를 포함하여 최종 실행: dynamic library 함수들은 컴파일 후 실행될 때 실행 코드를 가져옴

* interpretor
  - 소스코드를 한 행씩 번역하여 실행 (Javascript, BASIC 등)
  - 변수를 선언할 필요 없음
  - 장점: 실행이 편리
  - 사용 프로그램: 간단한 프로그램

* MMU(Memory Manage Unit)
  - 메모리 관리자는 정확히 말해 MMU라는 하드웨어
  - 작업: fetch, placement, replacement

* 작업
  - 가져오기 작업: 사용자가 요청하면 프로세스와 데이터를 메모리로 가져오는 작업
    - 정책: 언제 메모리로 가져올지 결정 (ex. prefetch)
  - 배치 작업: 가져온 프로세스와 데이터를 메모리 어떤 부분에 올려놓을지 결정하는 작업
    - 정책: paging, segmentation
  - 재배치 작업: 새로운 프로세스 가져오기 위해 꽉 찬 메모리에서 오래된 프로세스를 내보내는 작업
    - 정책: 어떤 프로세스를 내보낼지 결정. replacement algorithm

## 02 메모리 주소

* absolute address
  - 메모리 관리자 입장에서 바라본 주소. 메모리 주소 레지스터가 사용하는 주소. 실제 주소
  - physical address space에서 사용

* relative address
  - 사용자 프로세스 입장에서 사용자 영역이 시작되는 번지를 0번지로 변경하여 사용
  - logical address space에서 사용
  - 재배치 레지스터에 사용자 영역 시작 주소값이 저장되어 절대주소로 변환할 때 사용

## 03 단일 프로그래밍 환경에서의 메모리 할당

* memory overlay
  - 프로그램의 크기가 실제 메모리(물리 메모리)보다 클 때 몇 개의 모듈로 나누고 필요할 때마다 모듈을 메모리에 가져옴
  - 프로그램 전체를 메모리에 올려놓고 실행할 때보다 속도 느리지만, 메모리가 프로그램보다 작을 때도 실행 가능

* swap area
  - 메모리가 모자라서 쫓겨난 프로세스는 swap area에 모아둠
    - swap in: swap area -> memory
    - swap out: memory -> swap area

## 04 다중 프로그래밍 환경에서의 메모리 할당

* variable-size partitioning (가변 분할 방식) (= segmentation)
  - 프로세스의 크기에 따라 메모리를 나눔
  - contiguous memory allocation (연속 메모리 할당)
    - 장점: 하나의 프로세스를 연속된 공간에 배치
    - 단점: 메모리 관리가 복잡
  - external fragmentation
 
    <img src = "https://user-images.githubusercontent.com/23165155/110281725-97432180-8020-11eb-8dee-4ed4b9729082.png" width = "45%">
    
    - 해결책: memory placement strategy, defragmentation

  - memory placement strategy (메모리 배치 방식)
    - first fit (최초 배치): 첫 번째로 발견하는 공간에 프로세스를 배치
    - best fit (최적 배치): 빈 공간을 모두 확인 후 적당한 크기 가운데 가장 작은 공간에 프로세스 배치
    - worst fit (최악 배치): 빈 공간 모두 확인 후 가장 큰 공간에 프로세스 배치
  - defragmentation (조각 모음)
    - 서로 떨어져 있는 여러 개의 빈 공간을 합치는 작업
    - 시간 많이 걸림

* fixed-size partitioning (고정 분할 방식) (= paging)
  - 프로세스 크기와 상관없이 메모리를 같은 크기로 나눔
  - noncontiguous memory allocation (비연속 메모리 할당)
    - 장점: 메모리 관리가 수월
    - 단점: 메모리 낭비 발생
  - internal fragmentation

    <img src = "https://user-images.githubusercontent.com/23165155/110282647-2f8dd600-8022-11eb-82fa-a6c3d638211b.png" width = "50%">
    
    - 해결책: 신중하게 메모리의 크기를 결정해서 나눠야 함

* 버디 시스템
  - 외부 단편화를 완화. 그런데 고정 분할 방식과 유사
  - 프로세스의 크기에 맞게 메모리를 1/2 자르고 배치, 각 구역에는 프로세스 1개만 들어감, 프로세스 종료 시 빈 조각 합쳐짐
  - 특징
    - 내부 단편화 발생
    - 조각 모음 없이도 덩어리로 통합하기 쉬워서 효과적으로 공간 관리
