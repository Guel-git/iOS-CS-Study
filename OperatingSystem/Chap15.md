## Chap15. 파일 시스템

### 15-1 파일과 디렉터리

>**파일**

하드 디스크나 SSD와 같은 보조기억장치에 저장된 관련 정보의 집합

- 속성 (메타데이터): 파일 이름, 파일을 실행하기 위한 정보, 파일 관련 부가 정보
- 파일 속성: 파일 형식, 위치, 크기 등 파일과 관련된 다양한 정보
- 파일 유형: 운영체제가 인식하는 파일 종류 (ex. 확장자)
- 파일 연산을 위한 시스템 호출: 파일 생성/삭제/열기/닫기/읽기/쓰기

<br>

>**디렉터리**

파일들을 일목요연하게 관리하기 위한 방법으로, 윈도우에서는 폴더라고 부름

- 1단계 디렉터리: 모든 파일이 하나의 디렉터리 아래에 있는 구조
- 트리 구조 디렉터리: 여러 계층을 가진 디렉터리 구조
- 루트 디렉토리: 트리 구조에서 최상위 디렉터리 (/)
- 경로: 디렉터리를 이용해 파일 위치, 파일 이름을 특정 짓는 정보
    - 절대 경로: 루트부터 자기 자신까지 이르는 고유한 경로
    - 상대 경로: 현재 디렉터리부터 시작하는 경로
- 디렉터리 연산을 위한 시스템 호출: 디렉터리 생성/삭제/열기/닫기/읽기
- 디렉터리 엔트리: 디렉터리는 보조기억장치에 테이블 형태로 저장됨 ➡️ 엔트리만으로도 디렉터리 정보를 유추할 수 있음

<br>

### 15-2 파일 시스템

파일과 디렉터리를 보조기억장치에 일목요연하게 저장하고 접근할 수 있게 하는 운영체제 내부 프로그램

>**파티셔닝과 포매팅**

- 파티셔닝: 저장 장치의 논리적인 영역을 구획하는 작업, 칸막이로 영역을 나누는
- 포매팅: 파일 시스템을 설정하여 어떤 방식으로 파일을 저장하고 관리할 것인지 결정하고 새로운 데이터를 쓸 준비를 하는 작업

<br>

>**파일 할당 방법**

- 운영체제는 파일과 디렉터리를 블록 단위로 읽고 씀
- 파일을 보조기억장치에 할당하는 방법: 연속 할당, 불연속 할당
    - 연속 할당: 보조기억장치 내 연속적인 블록에 파일을 할당하는 방식  (시작 블록 주소, 블록 길이 / 외부 단편화)
    - 연결 할당 (불연속 할당): 각 블록에 다음 블록 주소를 저장하여 다음 블록을 가리키는 형태로 할당하는 방식 
        - 첫번째 블록부터 접근해야 함 ➡️ 임의의 위치까지 접근하는 속도 (임의 접근 속도)가 느림
        - 하드웨어 고장이나 오류 발생시 다음 블록 접근 불가능
    - 색인 할당: 색인 블록에 모든 블록 주소를 저장하여 관리하는 방식

<br>

>**파일 시스템 살펴보기**

- FAT (파일 할당 테이블) 파일 시스템
    - 각 블록에 포함된 다음 블록의 주소들을 한데 모아 테이블 형태로 관리하는 파일 시스템
    - USB 메모리, SD 카드에서 사용
    - 메모리에 캐시될 수 있음
      
      <img width="455" alt="스크린샷 2023-07-21 오전 8 44 37" src="https://github.com/Guel-git/iOS-CS-Study/assets/81340603/4a49dd8c-3528-4fa4-8ebd-0baaed60012d">

- 유닉스 파일 시스템
    - 색인 할당 기반으로 i-node 라는 색인 블록 사용
    - i-node: 파일 속성 정보와 15 개의 블록 주소를 저장할 수 있음
      
      <img width="455" alt="스크린샷 2023-07-21 오전 8 50 38" src="https://github.com/Guel-git/iOS-CS-Study/assets/81340603/75093bb9-8aed-47b5-9f53-802f4d4ffabb">

    - 한정적인 크기 ➡️ 12 개까지는 직접 블록 주소 저장, 13 번째는 단일 간접 블록 주소 저장 (데이터가 저장된 블록의 블록 주소), 14 번째 이중 간접 블록 주소 저장, 15 번째 삼중 간접 블록 주소 저장

      <img width="461" alt="스크린샷 2023-07-21 오전 8 54 29" src="https://github.com/Guel-git/iOS-CS-Study/assets/81340603/ec047316-4b17-4ad1-acdf-79c9e6980745">

- 이외에도 NT 파일 시스템, ext 파일 시스템 등이 있음

<br>

>**저널링 파일 시스템**

작업 로그를 통해 시스템 크래시가 발생했을 때 빠르게 복구하기 위한 방법, 로그만 확인하면 됨

1. 작업 직전 파티션의 로그 영역에 수행하는 작업에 대한 로그 남기기
2. 작업 수행
3. 로그 삭제