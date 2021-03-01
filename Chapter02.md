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
  - control bus (제어 버스): 어떤 작업 할지 지시하는 제어 신호가 다님. CPU, 메모리, 주변장치와 양방향으로 오감
  - address bus (주소 버스): 메모리의 데이터를 읽거나 쓸 때 주소가 다님.
  - data bus (데이터 버스)


## 03 컴퓨터 성능 향상 기술


## 04 병렬 처리
