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
  - 효율성: 시스템 자원이 유휴 시간 없이 사용되도록 하고, 유휴 자원 사용하려는 프로세스에는 우선권 부여
  - 안정성: 우선순위 배정함으로써 시스템 자원 점유, 파괴하려는 프로세스로부터 자원을 보호
  - 확장성: 프로세스가 증가해도 시스템이 안정적으로 작동하도록, 자원이 늘어나는 경우에도 혜택이 반영되도록
  - 반응 시간 보장: 응답이 없는 경우 시스템은 적절한 시간 안에 프로세스의 요구에 반응해야 함
  - 무한 연기 방지: 특정 프로세스의 작업이 무한히 연기되어서는 안됨

## 02 스케줄링 시 고려 사항

- preemptive scheduling (선점형 스케줄링): '빼앗을 수 있음'
  - 어떤 프로세스가 CPU를 할당받아 실행 중이더라도 OS가 강제로 빼앗을 수 있는 스케줄링 방식
  - 장점: 프로세스가 CPU 독점할 수 없어 대화형 시스템, 시분할 시스템에 적합
  - 단점: 문맥 교환의 오버헤드가 많음
  - 시분할 방식 스케줄러에 사용

- non-preemptive scheduling (비선점형 스케줄링): '빼앗을 수 없음'
  - 어떤 프로세스가 CPU를 점유하면 다른 프로세스가 이를 빼앗을 수 없는 스케줄링 방식
  - 장점: 스케줄러의 작업량이 적고 문맥 교환에 의한 오버헤드도 적음
  - 단점: 기다리는 프로세스가 많아 처리율이 떨어짐
  - 일괄 작업 장식 스케줄러에 사용

* 우선순위
  - 커널 프로세스 > 일반 프로세스 [명확]
  - 전면 프로세스 > 후면 프로세스 [명확]
  - 대화형 프로세스 > 일괄 처리 프로세스 [구분 불가한 경우도 有]
  - 입출력 집중 프로세스 > CPU 집중 프로세스 [구분 불가한 경우도 有]

## 03 다중 큐

- multiple queue: 프로세스를 효율적으로 관리하기 위해 큐를 여러 개 두어 관리
  - 준비 상태의 다중 큐
    - 우선순위에 따라 다중 큐 운영
  - 대기 상태의 다중 큐
    - 같은 입출력을 요구한 프로세스들을 모아 다중 큐 운영

## 04 스케줄링 알고리즘

<img src = "https://user-images.githubusercontent.com/23165155/109760631-5d45da00-7c32-11eb-9a21-7631745a20b0.PNG" width = "75%">

* FCFS(First Come First Service) scheduling
  - 준비 큐에 도착한 순서대로 CPU 할당. 큐 하나라 모든 프로세스의 우선순위가 동일함
  - 장점: 알고리즘이 단순하고 공평함
  - 단점: convoy effect(호위 효과)- 실행시간 긴 프로세스가 CPU 차지하면 다른 프로세스는 하염없이 기다려 효율성 떨어짐

* SJF(Shortest Job First) scheduling
  - 준비 큐에 있는 프로세스 중 실행 시간이 가장 짧은 작업부터 CPU 할당
  - 장점: convoy effect를 완화하여 시스템 효율성을 높임
  - 단점
    - OS가 프로세스 종료 시간을 정확하게 예측하기 어려움
    - starvation: aging을 통해 완화

* HRN(Highest Response Ratio Next) scheduling
  - 대기시간과 CPU 사용 시간을 고려하여 우선순위를 결정
  - 우선순위

    <img src = "https://user-images.githubusercontent.com/23165155/109764043-56b96180-7c36-11eb-9d70-2f9066f1a877.PNG" width = "30%">
    
  - starvation을 해결하기 위해 만들어짐.
* RR(Round Robin) scheduling
* SRT(Shortest Remaining Time) scheduling
* priority scheduling
* multilevel queue scheduling
* multilevel feedback queue scheduling
