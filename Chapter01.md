# Chapter 01 운영체제의 개요

## 01 운영체제 소개

* OS 관련 질문
  + 컴퓨터는 OS 없이도 작동하는가?
    - 작동하지만 기능에 제약이 따름
  + OS가 있는 기계와 없는 기계의 차이?
    - OS가 있으면 다양한 응용 프로그램을 설치/사용할 수도, 성능 향상을 위한 새로운 기능 추가도 가능
  + OS는 성능 향상하는 데에만 필요한가?
    - 자원을 관리하고 사용자에게 편리한 인터페이스 환경을 제공
  + OS는 자원을 어떻게 관리하는가?
    - OS는 사용자가 직접 자원에 접근하는 것을 막음으로써 컴퓨터 자원을 보호
  + 사용자는 숨어 있는 자원을 어떻게 이용할 수 있는가?
    - OS가 제공하는 사용자 인터페이스와 하드웨어 인터페이스를 이용하여 자원에 접근

* OS (Operating System)
  + 사용자에게 편리한 인터페이스 환경을 제공, 컴퓨터 시스템의 자원을 효율적으로 관리하는 소프트웨어
  
* OS의 역할과 목표
  - 자원 관리 <-> 효율성
  - 자원 보호 <-> 안정성
  - 하드웨어 인터페이스 제공 <-> 확장성
  - 사용자 인터페이스 제공 <-> 편리성

## 02 운영체제의 역사

<img src = "https://user-images.githubusercontent.com/23165155/109460285-afa4c080-7aa3-11eb-8be3-f01e8f7ef876.PNG" width="65%">

## 03 운영체제의 구조

<img src = "https://user-images.githubusercontent.com/23165155/109463026-b7666400-7aa7-11eb-8c66-5947b31ffc37.PNG" width="50%">

* kernel
  + 운영체제의 핵심적인 기능을 모아 놓은 것
  - system call: 응용 프로그램과 커널 사이의 인터페이스. 함수 형태 (ex. printf())
  - driver: 하드웨어와 커널 사이의 인터페이스

* interface
  - kernel에 사용자의 명령을 전달하고 실행 결과를 사용자에게 알려주는 역할

* kernel 종류
  + monolithic architecture kernel (단일형 구조 커널)
    - 
  + layered architecture kernel (계층형 구조 커널)
  + micro architecture kernel (마이크로 구조 커널)
