## Chap12. 프로세스 동기화

### 12-1 동기화란

>**동기화의 의미**

동시다발적으로 실행되는 많은 프로세스 > 서로 데이터를 주고받으며 공동의 목표를 위해 협력 > 동기화 필수

<br>

**프로세스 동기화란?**
- 프로세스들 사이의 수행 시기를 맞추는 것

<br>

**동기화의 종류**
1. 실행 순서 제어
-  프로세스를 올바른 순서대로 실행하기
- Write 이후에 Read가 실행될 수 있도록

2. 상호 배제
- 공유가 불가능한 자원의 동시 사용을 피하기 위해 사용하는 알고리즘
- ex. 두 개의 입금 프로세스에서 공유하는 '잔액'값의 상호배제 동기화

<br>

>**생산자와 소비자 문제**

- 생산자: 물건을 계속해서 생산하는 프로세스 (ex. +1)
- 소비자: 물건을 계속해서 소비하는 프로세스 (ex. -1)
- 생산자와 소비자가 동시에 실행될 때 동시에 사용하는 데이터가 기대한 값과 다르게 계산된 경우 > 생산자와 소비자 문제

<br>

>**공유 자원과 임계 구역**

- 공유 자원: 동시에 실행되는 프로세스들이 공동으로 작업하는 자원
- 임계 구역: 동시에 실행하면 문제가 발생하는 자원에 접근하는 코드 영역
- 두 개 이상의 프로세스가 임계 구역 접근시 하나는 대기
- 레이스 컨디션: 동시에 실행되면 안되는 영역에서 잘못된 실행으로 프로세스가 동시에 임계 구역 코드를 실행하여 데이터의 일관성이 깨지는 상황

  <img width="509" alt="스크린샷 2023-07-18 오전 3 23 35" src="https://github.com/Guel-git/iOS-CS-Study/assets/81340603/f72895f9-b4c7-4cda-ab6b-a3eea9f583e7">

<br>

>**상호 배제를 위한 동기화 원칙**

- 상호 배제: 임계 구역에 한 프로세스가 있다면 다른 프로세스는 접근 불가
- 진행: 임계 구역에 프로세스 X > 진입하고 싶은 프로세스는 들어갈 수 있어야 함
- 유한 대기: 임계 구역에 들어오기 위해 무한정 대기해서는 안됨

<br>

### 12-2 동기화 기법

>**뮤텍스 락**

상호 배제를 위한 동기화 도구로, 뮤텍스 락을 이용해 임계 구역에 프로세스가 있음을 알릴 수 있음

- 전역 변수 lock: 자물쇠 역할
- acquire 함수: 임계 구역을 잠그는 역할, 임계 구역 확인 및 lock을 true로 바꿈
- release 함수: 임계 구역의 잠금을 해제하는 역할, 작업 후 호출하여 lock을 false로 바꿈
- 바쁜 대기: 락을 쉴 새 없이 반복하며 확인해 보는 대기 방식 (acquire)

<br>

>**세마포어**

공유 자원이 여러 개 있는 상황에서도 적용이 가능한 동기화 도구

- 전역 변수 S: 임계 구역에 진입할 수 있는 프로세스의 개수를 나타냄
- wait 함수: 임계 구역에 들어가도 좋은지, 기다려야 할지를 알려주는 함수
- signal 함수: 기다리는 프로세스에게 접근 가능하다는 신호를 주는 함수
- 자원이 없을 경우, 프로세스 상태를 대기로 만듦 > 프로세스의 PCB를 세마포를 위한 대기 큐에 넣음 > 다른 프로세스의 작업이 끝나 signal 함수를 호출 > signal 함수는 대기 중인 프로세스를 대기 큐에서 제거함 > 프로세스  상태를 준비 상태로 변경하고 준비 큐롤 옮겨줌
- 세마포를 이용한 프로세스 순서 제어: 세마포 변수 S를 0으로, 먼저 실행할 프로세스 뒤에 signal을, 다음에 실행할 프로세스 앞에 wait을 붙힘

  <img width="226" alt="스크린샷 2023-07-18 오전 3 39 49" src="https://github.com/Guel-git/iOS-CS-Study/assets/81340603/8914d3db-ddf9-45b5-9884-f7b3c9803b3c">

<br>

>**모니터**

<img width="346" alt="스크린샷 2023-07-18 오전 3 41 04" src="https://github.com/Guel-git/iOS-CS-Study/assets/81340603/d20e46c9-091b-4c11-91f0-6fb197a4cdf1">

- 모니터라는 인터페이스를 통해서만 공유 자원 접근 허용
- 모니터 내부에는 항상 하나의 프로세스만 들어오도록 제어하는 큐 존재
- 조건 변수: 프로세스나 스레드의 실행 순서를 제어하기 위해 사용하는 특별한 변수
- 이미 진입한 프로세스의 실행 조건이 만족될 때까지 조건 변수에 대한 대기 큐에 삽입 (wait)
- 다른 프로세스의 signal 연산으로 실행이 재개됨

