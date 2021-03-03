# Chapter 04 CPU 스케줄링

## 01 스케줄링의 개요

* CPU scheduler (= process scheduler)
  - 프로세스가 생성된 후 종료될 때까지 모든 상태 변화를 조정
  - CPU와 시스템 자원을 어떻게 배정할지 결정

* 스케줄링의 단계
  * high level scheduling
    - 시스템 내의 전체 작업 수를 조절하는 것
    - = long-term scheduling = job scheduling
    - 어떤 작업을 시스템이 승인할지 거부할지를 결정 (= admission scheduling)
  * low level scheduling
    - 어떤 프로세스에 CPU를 할당할지, 어떤 프로세스를 대기 상태로 보낼지 등을 결정
    - 아주 짧은 시간에 일어남 (= short-term scheduling)
  * middle level scheduling
    - 전체 시스템의 활성화된 프로세스 수를 조절하여 과부하를 막는 것
    - 고수준 스케줄링과 저수준 스케줄링 사이에 일어남
    - suspend(중지)와 active(활성화)로 전체 시스템의 활성화된 프로세스 수를 조절
    - 일부를 suspend status로 옮겨(swap out) buffer의 역할을 함

* 스케줄링의 목적
  - 공평성: 자원 배정 과정에서 특정 프로세스가 배제되어서는 안됨
  - 효율성: 
  - 안정성
  - 확장성
  - 반응 시간 보장
  - 무한 연기 방지

## 02 스케줄링 시 고려 사항


## 03 다중 큐


## 04 스케줄링 
