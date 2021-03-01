# Chapter 02 컴퓨터의 구조와 성능 향상

## 01 컴퓨터의 기본 구성

* 하드웨어 구성
  - 필수장치: CPU, Main memory
  - 주변장치: 입력장치, 출력장치, 저장장치

* von Neumann architecture
  - "모든 프로그램은 메모리에 올라와야 실행할 수 있다."
  - CPU, 메모리, 입출력장치, 저장장치가 버스로 연결되어 있는 구조

## 02 CPU와 메모리

* CPU 구성
  - ALU (Arithmetic and Logic Unit)
    + 산술 연산과 논리 연산을 수행
  - control unit
    + 작업을 지시하는 부분
  - register
    + 데이터를 임시로 보관하는 곳

* register 종류
  + user-visible register
    - DR (Data Register): 메모리에서 가져온 데이터를 보관
    - AR (Address Register): 데이터 또는 명령어가 저장된 메모리의 주소를 보관
  + user-invisible register (특수 레지스터)
    - PC (Program Counter): 다음에 실행할 명령어 주소를 보관
    - IR (Instruction Register): 현재 실행중인 명령어를 보관
    - MAR (Memory Address Register): 관리자가 접근해야 할 메모리의 주소를 저장
    - MBR (Memory Buffer Register): 메모리 관리자가 메모리에서 가져온 데이터를 임시로 저장

* bus 종류
  
  <img src = "https://user-images.githubusercontent.com/23165155/109487358-741bed80-7ac7-11eb-8912-fb363f4bed3b.PNG" width="60%">
  
  - control bus (제어 버스): 어떤 작업 할지 지시하는 제어 신호가 다님 [양방향]
  - address bus (주소 버스) [단방향]
  - data bus (데이터 버스) [양방향]

* memory 종류
  - RAM (Random Access Memory)
  - RON (Read Only Memory)

* memory 보호
  - bound register (경계 레지스터): 현재 진행 중인 작업의 메모리 시작 주소
  - limit register (한계 레지스터): 현재 진행중인 작업이 차지하는 메모리의 크기 (시작 주소 - 마지막 주소)

* booting
  - 컴퓨터를 켰을 때 OS를 메모리에 올리는 과정
  - 전원 ON-> ROM에 저장된 BIOS 실행되어 하드웨어 점검-> 메모리에 bootstrap code 올려 실행-> OS를 메모리로 가져와 실행

## 03 컴퓨터 성능 향상 기술

* buffer
  - 일정량의 데이터를 모아 한꺼번에 전송하여 속도 차이 있는 두 장치 사이 차이를 완화

* cache
  - 메모리와 CPU 간 속도 차이를 완화하기 위해 메모리에서 사용할 것으로 예상되는 데이터를 미리 가져와 저장해두는 임시 장소

* storage hierarchy

  <img src = "https://user-images.githubusercontent.com/23165155/109492312-1f2fa580-7ace-11eb-91ba-9924a56ee521.PNG" width="50%">

  - 속도 빠르고 값이 비싼 저장장치를 CPU 가까운 쪽에 두고, 값 싸고 용량 큰 저장장치를 반대쪽에 배치
  - 적당한 가격으로 빠른 속도와 큰 용량을 동시에 얻는 방법

## 04 병렬 처리
