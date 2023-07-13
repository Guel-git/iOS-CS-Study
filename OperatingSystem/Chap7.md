## Chap7. 보조기억장치

### 7-1 다양한 보조기억장치

<br>

>**하드 디스크**

자기적인 방식으로 데이터를 저장하는 보조기억장치 (aka 자기 디스크)

<img>

- 플래터: 데이터가 저장되는 동그란 원판,  수많은 N극과 S극 저장
- 스핀들: 플래터를 회전시키는 구성 요소, RPM(분당 회전 수)

<img>

- 헤드: 플래터를 대상으로 데이터를 읽고 쓰는 구성요소, 바늘처럼 생김
- 디스크 암: 원하는 위치로 헤드를 이동시키는 요소
- 여러 겹의 플래터로 구성되고 플래터의 양면 모두 사용 가능

<img>

- 트랙: 플래터를 구성하는 하나의 원
- 섹터: 피자 조각처럼 트랙을 나눴을 때 나오는 조각 하나, *가장 작은 전송 단위*
- 블록: 하나 이상의 섹터들의 묶음
- 실린더: 여러 겹의 플래터에서 같은 트랙 위치를 모아 연결한 논리적 단위, 연속된 정보 저장

<br>

**데이터 접근 시간**

- 탐색 시간: 헤드 이동 시간
- 회전 지연: 플래터 회전 시간
- 전송 시간: 디스크 <-> 컴퓨터 데이터 전송 시간
- 탐색 시간 & 회전 지연 단축 ? 플래터 회전 속도 올리기, 참조 지역성 고려 필요

<br>

**헤드 디스크 종류**
- 다중 헤드 디스크 = 고정 헤드 디스크
- 단일 헤드 디스크 = 이동 헤드 디스크

<br>

>**플래시 메모리**

전기적으로 데이터를 읽고 쓸 수 있는 반도체 기반 저장 장치 (USB, SD, SSD 등)

- 셀: 데이터를 저장하는 가장 작은 단위
- 하나의 셀에 몇 비트를 저장할 수 있느냐에 따라 종류 달라짐

<br>

**SLC 타입**

- 셀 1개당 1비트 저장 > 2개의 입출력
- 수명이 길지만 용량 대비 가격이 높음
- 데이터 R/W 가 많으며 빠른 저장 장치로 사용

<br>

**MLC 타입**

- 셀 1개당 2비트 저장 > 4개의 입출력
- 속도와 수명 떨어짐, 용량 대비 가격 저렴
- 시중에서 사용되는 일반적인 타입

<br>

**TLC 타입**

- 셀 1개당 3비트 저장 > 8개의 입출력
- 속도와 수명 낮음, 용량 대비 가격 가장 저렴

<br>

**플래시 메모리의 구성**

- 셀 > 페이지 > 블록 > 플레인 > 다이
- 읽기/쓰기는 페이지 단위, 삭제는 블록 단위
- 페이지의 상태
    - Free: 데이터가 없는 상태
    - Valid: 유효한 데이터를 저장하고 있는 상태, 추가 데이터 저장 불가능
    - Invalid: 유효하지 않은 데이터를 저장하고 있는 상태
- 덮어쓰기가 불가능하여 유효하지 않는 데이터가 공간 차지함, 하지만 삭제는 블록 단위로만 가능
    - 가비지 컬렉션: 유효한 페이지들만 새로운 블록으로 복사 > 기존 블록 삭제

<br>

### 7-2 RAID의 정의와 종류

<br>

>**RAID의 정의**

데이터의 안전성 혹은 높은 성능을 위해 여러 개의 물리적 보조기억장치를 마치 하나의 논리적 보조기억장치처럼 사용하는 기술

>**RAID의 종류**

RAID 구성 방법 > RAID 레벨

**RAID 0**

- 여러 개의 보조기억장치에 데이터를 단순히 나누어 저장하는 구성 방식
- 각 하드 디스크는 번갈아 가며 데이터 저장
- 데이터를 동시에 읽고 쓸 수 있음
- 정보가 안전하게 저장되지 않음

<img>

- 스트라입: 줄무늬처럼 분산되어 저장된 데이터
- 스트라이핑: 분산하여 저장하는 것

**RAID 1**

<img>

- 미러링: 거울처럼 완전한 복사본(백업)을 만듦
- 문제 발생시 복구가 간단함
- 속도가 느리고 사용 가능 용량이 적어짐 > 많은 디스크 필요, 비용 증가


**RAID 4**

<img>

- 복사본 대신 오류 검출 및 복구를 위한 저장 장치로 구성
- 패리티 비트: 오류를 검출하고 복구하기 위한 정보
- 적은 하드 디스크로 데이터를 안전하게 보관

**RAID 5**

<img>

- RAID 4에서 패리티 저장 장치에 병목 현상 발생함 > 패리티 정보 분산


**RAID 6**

<img>

- 패리티 정보를 분산하며, 서로 다른 두 개의 패리티를 두는 방식
- 오류 검출/복구 수단이 2배가 됨 > 안전한 구성
- 쓰기 속도가 느림