# Section 3.2 메모리

## 3.2.1 메모리 계층

- 레지스터: CPU 안에 있는 작은 메모리. 휘발성, 속도 가장 빠름, 기억용량 가장 적음
- 캐시: L1, L2 캐시. 휘발성, 속도 빠름, 기억용량 적음
- 주기억장치: RAM. 휘발성, 속도 보통, 용량 보통
- 보조기억장치: HDD, SSD. 비휘발성, 속도 낮음, 용량 많음

RAM은 하드디스크로부터 일정량의 데이터를 복사해서 임시 저장하고, CPU에게 빠르게 전달하는 역할.</br>
계층 - 경제성, 캐시</br>
올라갈수록 가격 ↑ 용량 ↓ 속도 ↑(빨라짐)</br>
Ex. ‘로딩 중’ → 하드디스크 또는 인터넷에서 데이터를 읽어 RAM으로 전송 중

### 캐시(Cache)

- 데이터를 미리 복사해 놓는 임시 저장소
- 빠른 장치와 느린 장치에서 속도 차이에 따른 병목 현상을 줄이기 위한 메모리
- 캐싱 계층: 계층과 계층 사이의 속도 차이를 해결하기 위해 존재하는 계층
ex. 캐시 메모리와 보조기억장치 사이에 있는 주기억장치를 `보조기억장치의 캐싱계층` 이라 할 수 있다.

지역성의 원리

- 캐시를 직접 설정시 → 자주 사용하는 데이터를 기반으로 설정
- 자주 사용한다의 기준은? 지역성!

  ```swift
  var arr = [Int](repeating: 0, count: 10)
  print(arr) // [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
  
  for i in 0..<10 {
  	arr[i] = i
  }
  print(arr) // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
  ```

- 시간 지역성(temporal locality)
    - 최근 사용한 데이터에 다시 접근하려는 특성
    - 코드에서 `i`
- 공간 지역성(spatial locality)
    - 최근 접근한 데이터를 이루고 있는 공간이나 그 가까운 공간에 접근하는 특성
    - 코드에서 `arr`

### 캐시히트와 캐시미스

- 캐시히트
    - 캐시에서 원하는 데이터를 찾았다.
    - 제이장치를 거쳐서 가져옴
    - 위치도 가깝고 CPU 내부 버스를 기반으로 작동 → 빠르다
- 캐시미스
    - 해당 데이터가 캐시에 없다.
    - 주메모리로 가서 데이터를 찾아와야함
    - 시스템 버스 기반으로 작동 → 느리다
- 캐시매핑
    - 캐시가 히트되기 위해 매핑하는 방법
    - CPU의 레지스터와 RAM 간에 데이터 주고 받을 때
        - 레지스터는 굉장히 작고, RAM은 굉장히 크다.(상대적으로)
        - 작은 레지스터가 캐시 계층으로써 역할을 잘해주려면 매핑이 중요하다!!</br>
        
      |이름|설명|
      |:--:|--|
      |직접 매핑<br>(Directed Mapping)| - 메모리가 1~100, 캐시가 1~10 → 1:1~10, 2:1~20 이런식으로 매핑 <br> - 처리가 빠르지만 충돌 발생이 잦다. |
      |연관 매핑<br>(Associative Mapping)| - 순서와 상관없이 관련 있는 캐시와 메모리를 매핑 <br> - 충돌이 적지만 속도가 느리다(모든 블록을 탐색해야함). |
      |집합 연관 매핑<br>(Set Associative Mapping)| - 직접 매핑 + 연관 매핑 <br> - 순서는 일치시키지만 집합을 둬서 저장. <br> - 블록화 → 검색에 효율적 |
- 웹 브라우저의 캐시
    - 쿠키, 로컬 스토리지, 세션 스토리지
    - 보통 사용자의 커스텀한 정보나 인증 모듈 관련 사항들을 웹브라우저에 저장
    추후 서버에 요청할 때 자신을 나타내는 아이덴티티나 중복 요청 방지를 위해 쓰임
    - 쿠키
        - 만료기한이 있는 key-value 저장소
        - 쿠키 설정 시 document.cookie로 쿠키를 볼 수 없게 httponly 옵션을 거는 것이 중요.
        - 클라이언트 또는 서버에서 만료기한 등을 설정 가능. 보통은 서버에서.
    - 로컬 스토리지
        - 만료기한이 없는 key-value 저장소
        - 10MB까지 저장 가능
        - 웹 브라우저 닫아도 유지. 도메인 단위로 저장, 생성.
        - HTML5 지원 하는 웹에서 사용 가능
        - 클라이언트에서만 수정 가능
    - 세션 스토리지
        - 만료기한이 없는 key-value 저장소
        - 5MB까지 저장 가능
        - 탭 단위로 세션 스토리지 생성. 탭 닫을 때 데이터 삭제.
        - HTML5 지원 하는 웹에서 사용 가능
        - 클라이언트에서만 수정 가능

- 데이터베이스의 캐싱 계층
    - 데이터베이스 시스템을 구축할 때도 메인 데이터 베이스 위에 레디스(redis) 데이터베이스 계층을 캐싱 계층으로 둬서 성능을 향상 시키기도 한다.
        
## 3.2.2 메모리 관리

### 가상 메모리(virtual memory)

- 메모리 관리 기법의 하나.
컴퓨터가 실제로 이용 가능한 메모리 자원을 추상화.
사용자들에게 매우 큰 메모리로 보이게 만드는 것.
- 가상주소(logical address)와 실제주소(physical address)가 매핑 되어 있다.
- 가상주소는 메모리관리장치(MMU)에 의해 실제 주소로 변환
(사용자는 실제 주소를 의식할 필요 없이 프로그램 구축 가능)
- 프로세스의 주소 정보가 들어 있는 `페이지 테이블`로 관리 된다.

<aside>
💡 TLB
메모리와 CPU 사이에 있는 주소 변환을 위한 캐시.
페이지 테이블에 있는 리스트를 보관하며 CPU가 페이지 테이블까지 가지 않도록 해 속도를 향상 시킬 수 있는 캐시 계층.
</aside> <br>
 <br>
- 스와핑(swapping)
    - 페이지폴트 발생시 메모리에서 당장 사용하지 않는 영역을 하드디스크로 옮기고 하드디스크의 일부분을 마치 메모리처럼 불러와 쓰는 것
    - 이를 통해 마치 페이지 폴트가 일어나지 않은 것처럼 만든다
- 페이지폴트(page fault)
    - 가상 메모리에는 존재하지만 실제 메모리인 RAM에는 현재 없는 데이터나 코드에 접근할 경우 발생
- 페이지 폴트와 스와핑 과정
    1. CPU - 물리 메모리를 확인하여 해당 페이지가 없으면 트랩을 발생해서 운영체제에 알림
    2. 운영체제 - CPU의 동작을 잠시 멈춤
    3. 운영체제 - 페이지 테이블을 확인하여 가상 메모리에 페이지가 존재하는지 확인하고, 없으면 프로세스 중단 후 현재 물리 메모리에 비어있는 프레임이 있는지 탐색.
    물리 메모리에도 없다면 스와핑 발동
    4. 비어있는 프레임에 해당 페이지 로드하고, 페이지 테이블을 최신화
    5. 중단되었던 CPU 다시 시작.

<aside>
💡 페이지(page): 가상 메모리를 사용하는 최소 크기 단위
프레임(frame): 실제 메모리를 사용하는 최소 크기 단위

</aside>

### 스레싱(thrashing)
- 메모리에 너무 많은 프로세스가 동시에 올라가게 되면 스와핑이 많이 일어나서 발생
- 메모리의 페이지 폴트율이 높음을 의미. → 컴퓨터의 심각상 성능 저하를 초래
- 페이지폴트 발생 → CPU 이용률 저하 → “CPU가 한가한가?” 가용성을 높이기 위해 더 많은 프로세스를 메모리에 올림 → 악순환 → 스레싱
- 해결방법
    - 메모리를 늘린다
    - HDD를 SSD로 바꾼다.
    - 운영체제 - 작업세트, PFF
- 작업세트(working set)
    - 프로세스의 과거 사용 이력인 지역성(locality)을 통해 결정된 페이지 집합을 만들어서 미리 메모리에 로드
    - 탐색에 드는 비용을 줄일 수 있고, 스와핑 또한 줄일 수 있다.
- PFF(Page Fault Frequency)
    - 페이지 폴트 빈도를 조절하는 방법 - `상한선`과 `하한선`을 만든다.
    - 상한선에 도달 - 프레임을 늘린다.
    하한선에 도달 - 프레임을 줄인다.

### 메모리 할당

시작 메모리 위치, 메모리의 할당 크기를 기반으로 할당한다. <br>
연속 할당과 불연속 할당으로 나뉨

- 연속 할당
    - 메모리에 연속적으로 공간을 할당하는 것
    - 고정 분할 방식(fixed partition allocation)
        - 메모리를 미리 나누어 관리
        - 융통성이 없다.
        - 내부 단편화 발생
    - 가변 분할 방식(variable partition allocation)
        - 매 시점 프로그램의 크기에 맞게 동적으로 메모리를 분할하여 사용
        - 내부 단편화 발생 X. but 외부 단편화 발생 가능
        - 종류
            | 최초 적합(first fit) | 위쪽이나 아래쪽부터 시작해서 홀을 찾으면 바로 할당 |
            | --- | --- |
            | 최적 적합(best fit) | 프로세스의 크기 이상인 공간 중 가장 작은 홀부터 할당 |
            | 최악 적합(worst fit) | 프로세스의 크기와 가장 많이 차이가 나는 홀에 할당 |
    
    <aside>
    💡 내부 단편화(internal fragmentation): 메모리를 나눈 크기보다 프로그램이 작아서 들어가지 못하는 공간이 많이 발생하는 현상
    
    </aside>
    
    <aside>
    💡 외부 단편화(external fragmentation): 메모리를 나눈 크기보다 프로그램이 커서 들어가지 못하는 공간이 많이 발생하는 현상
    
    </aside>
    
    <aside>
    💡 홀(hole): 할당할 수 있는 비어있는 메모리 공간
    
    </aside>
    
- 불연속 할당
    - 메모리를 연속적으로 할당하지 않는다. 현대 운영체제가 쓰는 방법
    - 페이징(paging) 기법
        - 메모리를 동일한 크기의 페이지(보통 4KB)로 나누어 메모리의 서로 다른 위치에 프로세스를 할당
        - 프로그램마다 페이지 테이블을 두어 이를 통해 메모리에 프로그램을 할당
        - 홀의 크기가 균일하지 않은 문제가 없어진다.
        - 주소 변환이 복잡해진다.
    - 세그멘테이션(segmentation)
        - 의미 단위인 세그먼트(segment)로 나누는 방식
        - 코드와 데이터 등 프로세스의 구성요소를 기반으로 나눌 수 있고, 함수 단위로도 나눌 수 있다.
        - 공유와 보안 측면에서 좋다
        - 홀 크기가 균일하지 않은 문제가 생긴다.
    - 페이지드 세그멘테이션(paged segmentation)
        - 공유나 보안 - 의미 단위인 세그먼트로 나누고
        물리적 메모리 - 페이지로 나눈다.

### 페이지 교체 알고리즘

- 오프라인 알고리즘(offline algorithm)
    - 먼 미래에 참조되는 페이지와 현재 할당하는 페이지를 바꾸는 알고리즘
    - 가장 좋은 방법 but 미래에 사용되는 프로세스를 우리가 알 수 없다. → 사용 불가 알고리즘
    - 다른 알고리즘과의 성능 비교에 대한 기준을 제공
- FIFO(First In First Out)
    - 가장 먼저 온 페이지를 교체 영역에 가장 먼저 놓는 방법
- LRU(Least Recently Used)
    - 참조가 가장 오래된 페이지를 교체
    - ‘오래된’ 것을 파악하기 위해 각 페이지마다 계수기, 스택을 두어야 하는 문제점
    - 5번째에 5번 페이지가 들어왔을 때 가장 오래된 1번 페이지와 스왑
    - LRU를 프로그래밍으로 구현시 보통 두 개의 자료 구조로 구현
        - 해시테이블: 이중 연결 리스트에서 빠르게 찾을 수 있도록
        - 이중 연결 리스트: 한정된 메모리를 나타냄
- NUR(Not Used Recently)
    - LRU에서 발전. 일명 clock 알고리즘
    - 먼저 0과 1을 가진 비트를 둔다. 1은 최근에 참조, 0은 참조되지 않음
    - 시계 방향으로 돌면서 0을 찾고 0을 찾은 순간 해당 프로세스를 교체, 0은 1로 바꿈
- LFU(Least Frequently Used)
    - 가장 참조 횟수가 적은 페이지를 교체 (많이 사용되지 않은 것을 교체)
