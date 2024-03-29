# 메모리

<br>

## 📍메모리 계층
(위로 갈수록 속도가 빠름. 아래로 갈수록 용량이 큼)
- **레지스터** - CPU 안에 있는 작은 메모리
    - 휘발성, 속도 가장 빠름, 기억 용량이 가장 적음
- **캐시** - L1, L2, L3
    - 휘발성, 속도 빠름, 기억 용량이 적음
- **주기억장치** - RAM
    - 휘발성, 속도 보통, 기억 용량 보통
    - 하드디스크로부터 일정량의 데이터를 복사해서 임시 저장하고 이를 필요 시마다 CPU에 빠르게 전달하는 역할을 함
- **보조기억장치** - HDD, SSD
    - `비휘발성`, 속도 낮음, 기억 용량 많음

### 🔎 캐시(cache)
- 데이터를 미리 복사해 놓는 임시 저장소
- 빠른 장치와 느린 장치에서 **속도 차이에 따른 병목 현상을 줄이기 위한** 메모리
- 실제로 메모리와 CPU 사이의 속도 차이가 너무 크기 때문에 그 중간에 레지스터 계층을 둬서 속도 차이를 해결

<aside>
💡 캐싱 계층: 속도 차이를 해결하기 위해 계층과 계층 사이에 있는 계층
</aside>

#### 지역성의 원리
- 자주 사용하는 데이터에 대한 근거가 되는 것이 바로 지역성
    - **시간 지역성**: `최근 사용한` 데이터에 다시 접근하려는 특성
    - **공간 지역성**: `최근 접근한 데이터를 이루고 있는 공간이나 그 가까운 공간`에 접근하는 특성

<br>

### 🔎 캐시히트와 캐시미스
#### **캐시히트**
- `캐시`에서 원하는 데이터를 찾은 것
- 캐시히트의 경우 위치도 가깝고 CPU 내부 버스를 기반으로 작동하기 때문에 빠름

#### **캐시미스**
- 해당 데이터가 **캐시에 없다면** `주 메모리`로 가서 데이터를 찾아오는 것
- 캐시미스가 발생되면 메모리에서 가죠오게 되는데, 이는 시스템 버스를 기반으로 작동하기 때문에 느림

#### 캐시 매핑
- 캐시가 히트되기 위해 매핑하는 방법
- CPU와 레지스터와 주 메모리(RAM)간에 데이터를 주고받을 때를 기반으로 설명

| 이름 | 설명 |
| :---: | :--- |
| 직접 매핑<br>(directed mapping) | - 메모리가 1~100이 있고 캐시가 1~10이 있다면 1:1~10, 2:11~20 … 이런 식으로 매핑하는 것을 말함<br> - 처리가 빠르지만 충돌 발생이 잦음 |
| 연관 매핑<br>(associative mapping) | - 순서를 일치시키지 않고 관련있는 캐시와 메모리를 매핑<br> - 충돌이 적지만 모든 블록을 탐색해야해서 속도가 느림 |
| 집합 연관 매핑<br> (set associative mapping) | - 직접 매핑 + 연관 매핑<br> - 순서는 일치시키지만 집합을 둬서 저장하며 블록화되어 있기 때문에 검색은 효율적임 |

#### 웹 브라우저의 캐시
- 소프트웨어적인 대표적인 캐시로는 웹 브라우저의 작은 저장소 쿠키, 로컬 스토리지, 세션 스토리지가 있음
- 이러한 것들은 보통 사용자의 커스텀한 정보나 인증 모듈 관련 사항들을 웹 브라우저에 저장해서 추후 서버에 요청할 때 자신을 나타내는 아이덴티티나 중복 요청 방지를 위해 쓰임
- **쿠키**
    - 만료기한이 있는 키-값 저장소
    - 4KB까지 데이터 저장 가능, 만료 기한을 정할 수 있음
- **로컬 스토리지**
    - 만료기한이 없는 키-값 저장소
    - 10MB까지 저장할 수 있으며, 웹브라우저를 닫아도 유지되고 도메인 단위로 저장, 생성됨
- **세션 스토리지**
    - 만료기한이 없는 키-값 저장소
    - 탭 단위로 세션 스토리지를 생성하며, 탭을 닫을 때 해당 데이터가 삭제됨

#### 데이터베이스의 캐싱 계층
- 메인 데이터베이스 위에 `레디스(redis)` 데이터베이스 계층을 캐싱 계층으로 둬서 성능을 향상 시키기도함

<br>

## 📍메모리 관리
### 🔎 가상 메모리(virtual memory)
- 컴퓨터가 실제로 이용 가능한 메모리 자원을 `추상화`하여 이를 사용하는 사용자들에게 **매우 큰 메모리**로 보이게 만드는 것. RAM의 부족한 용량을 보완하는 데 주로 쓰임
- 가상 주소는 `메모리관리장치(MMU)`에 의해 실제 주소로 변환되며, 사용자는 실제 주소를 의식할 필요 없이 프로그램을 구축할 수 있음
    - **가상 주소(logical address)**: 가상적으로 주어진 주소
    - **실제 주소(physical address)**: 실제 메모리상에 있는 주소
- 가상 메모리는 가상 주소와 실제 주소가 매핑되어 있고 프로세스의 주소 정보가 들어 있는 ‘페이지 테이블’로 관리됨. 이 때 속도 향상을 위해 TLB 사용
    - **TLB**
        - 메모리와 CPU 사이에 있는 주소 변환을 위한 캐시
        - 페이지 테이블에 있는 리스트를 보관하며 CPU가 페이지 테이블까지 가지 않도록하여 속도를 향상시킬 수 있는 캐시 계층

#### 페이지 폴트(page fault)와 스와핑(swapping)
운영체제는 가동되고 있는 프로세스들의 내용(페이지) 중, 덜 중요한 것들을 하드 디스크의 공간에 옮겨 놓는다. 그리고 프로세스가 동작하는 도중, 메모리에 필요한 데이터(페이지)가 없으면 하드디스크를 찾아 해당 데이터를 가져온다.(이 과정에서 속도 저하 발생)

운영체제의 스와퍼(Swapper)는 물리 메모리에 동작하고 있는 모든 프로세스를 로드하지 않는다. 게다가 운영체제의 페이저(Pager)는 프로세스의 모든 페이지를 물리 메모리에 로드하지 않는다. 따라서 페이지가 물리 메모리에 부재할 수 있는데, 이것을 `페이지 폴트`라고 한다.

페이지 폴트가 발생하면 해당 페이지를 가상 메모리에서 찾아야한다. 이 때 운영체제가 페이지 폴트를 해결하는 과정을 요구 페이징이라고 한다. 

먼저 운영체제는 페이지 테이블로 가상 메모리를 관리한다. 페이지 테이블에는 각 페이지가 저장되어 있는 주소값이 들어있다. 게다가, 페이지 테이블에는 Valid bit가 있어, 이를 이용해 해당 페이지가 어느 메모리에 있는지 표시할 수 있다. 그러므로 운영체제는 페이지 테이블로 가상 메모리에서 페이지를 쉽게 찾을 수 있음

- **페이지 폴트**
    - 가상 메모리에는 존재하지만 `실제 메모리(RAM)에는 없는` 데이터나 코드에 접근할 경우 발생
    - 해당 페이지가 물리 메모리에 없는 상태이므로 운영체제는 페이지 폴트 인터럽트를 발생시켜 해당 페이지를 물리 메모리에 로드함
- **스와핑**
    - 현재 올라와있는 프로세스 중 일부를 하드 디스크로 옮겨놓고, 그 자리에 다른 프로세스를 올리는 작업. 이를 통해 마치 페이지 폴트가 일어나지 않은 것처럼 만든다.
- **요구 페이징을 수행하는 과정 - 해당 페이지를 가상 메모리에서 찾아 물리 메모리에 로드**
    1. CPU는 물리 메모리를 확인하여 해당 페이지가 없으면 트랩(trap)을 발생해서 운영체제에 알림
    2. 운영체제는 CPU의 동작을 잠시 멈춤
    3. 운영체제는 페이지 테이블을 확인하여 가상 메모리에 페이지가 존재하는지 확인
        1. 없으면 프로세스를 중단하고 현재 물리 메모리에 비어 있는 프레임(Free Frame)이 있는지 찾음
        2. 물리 메모리에 여유가 없다면 스와핑 발동
            - 물리 메모리에 비어있는 프레임이 없으면 어떻게 될까? 프로세스를 멈출 수는 없으므로, 희생 프레임을 골라서 이를 가상 메모리에 저장 후 필요한 페이지를 물리 메모리에 로드한다. 이 과정에서 페이지 교체 알고리즘이 사용됨
    4. 비어 있는 프레임에 해당 페이지를 로드하고, 페이지 테이블을 최신화함
    5. 중단되었던 CPU를 다시 시작

<aside>
💡 페이지(page): 가상 메모리를 사용하는 최소 크기 단위<br>
💡 프레임(frame): 실제 메모리를 사용하는 최소 크기 단위
</aside>

<br>

### 🔎 스레싱(thrashing)
- 메모리의 페이지 폴트율이 높은 것을 의미. 이는 컴퓨터의 심각한 성능 저하를 초래함
- 스레싱은 메모리에 너무 많은 프로세스가 동시에 올라가게 되면 스와핑이 많이 일어나서 발생
    - 페이지 폴트가 일어나면 CPU 이용률이 낮아짐
    - CPU 이용률이 낮아지면 → 운영체제는 “CPU가 한가한가?”하고 가용성을 높이기 위해 더 많은 프로세스를 메모리에 올리게됨
    - 악순환이 반복되며 스레싱 발생
- 해결방법
    - 메모리 늘리기
    - HDD를 사용한다면 SSD로 교체
    - 운영체제에서 - 작업 세트와 PFF
#### 작업 세트(working set)
- 프로세스의 과거 사용 이력인 **지역성(locality)을 통해 결정된 페이지 집합**을 만들어서 미리 메모리에 로드하는 것
- 미리 메모리에 로드하면 탐색에 드는 비용을 줄일 수 있고 스와핑 또한 줄일 수 있음
#### PFF(Page Fault Frequency)
- 페이지 폴트 빈도를 조절하는 방법으로 상한선과 하한선을 만드는 방법
- 상한선에 도달한다면 프레임을 늘리고 하한선에 도달한다면 프레임을 줄이는 것

<br>

### 🔎 메모리 할당
- 메모리에 프로그램을 할당할 때는 시작 메모리 위치, 메모리의 할당 크기를 기반으로 할당하는데, 연속 할당과 불연속 할당으로 나뉨

#### 연속 할당
- 프로세스를 메모리에 ‘연속적(순차적)으로’ 공간을 할당하는 것
- **고정 분할 방식(fixed partition allocation)**
    - 메모리를 미리 나누어 관리하는 방식
    - 메모리가 미리 나뉘어 있기 때문에 융통성이 없고 내부 단편화 발생
- **가변 분할 방식(variable partition allocation)**
    - 매 시점 프로그램의 크기에 맞게 동적으로 메모리를 나눠 사용
    - 내부 단편화 발생하지 않고, 외부 단펴화 발생할 수 있음
    - 가변 분할 방식 종류
        | 이름 | 설명 |
        | :---: | :--- |
        | 최초 적합<br>(first fit) | 위쪽이나 아래쪽부터 시작해서 홀을 찾으면 바로 할당 |
        | 최적 적합<br>(best fit) | 프로세스의 크기 이상인 공간 중 가장 작은 홀부터 할당 |
        | 최악 적합<br>(worst fit) | 프로세스의 크기와 가장 많이 차이가 나는 홀에 할당 |

<aside>
💡 내부 단편화(internal fragmentation) : 메모리를 나눈 크기보다 프로그램이 작아서 들어가지 못하는 공간이 많이 발생하는 현상
</aside>
<aside>
💡 외부 단편화(external fragmentation) : 메모리를 나눈 크기보다 프로그램이 커서 들어가지 못하는 공간이 많이 발생하는 현상
</aside>

#### 비연속 할당
- 메모리를 연속적으로 할당하지 않는 할당으로 현대 운영체제가 쓰는 방법으로 불연속 할당인 페이징 기법이 있음
- **페이징(Paging)**
    - `동일한 크기`의 페이지 단위로 나누어 메모리의 서로 다른 위치에 프로세스를 할당
    - 홀의 크기가 균일하지 않은 문제가 없어지지만 주소 변환이 복잡해짐
- **세그멘테이션(Segmentation)**
    - 페이지 단위가 아닌 의미 단위인 세그먼트(segment)로 나누는 방식
    - 프로세스는 코드, 데이터, 스택, 힙 등으로 이루어지는데, 코드와 데이터 등 이를 기반으로 나눌 수도 있으며 함수 단위로 나눌 수도 있음을 의미
    - 공유와 보안 측면에서 좋으며 홀 크기가 균일하지 않은 문제가 발생함
- **페이지드 세그멘테이션(paged segmentation)**
    - 공유나 보안을 의미 단위로 세그먼트로 나누고, 물리적 메모리는 페이지로 나누는 것

<br>

### 🔎 페이지 교체 알고리즘
- 메모리는 한정되어 있기 때문에 스와핑이 많이 일어남
- 스와핑이 많이 일어나지 않도록 설계되어야 하며 이는 페이지 교체 알고리즘을 기반으로 스와핑이 일어남

#### 1. 오프라인 알고리즘(offline algorithm)
- 먼 미래에 참조되는 페이지와 현재 할당하는 페이지를 바꾸는 알고리즘으로 가장 좋은 방법임
- 하지만 미래에 사용되는 프로세스를 우리가 알 수 있는 방법은 없음
- `사용할 수 없는 알고리즘`이지만 다른 알고리즘의 성능 비교에 대한 기준을 제공

#### 2. FIFO(First In First Out)
- `가장 먼저 온` 페이지를 교체 영역에 가장 먼저 놓는 방법

#### 3. LRU(Least Recently Used)
- `참조가 가장 오래된` 페이지를 바꿈
- ‘오래된’ 것을 파악하기 위해 각 페이지마다 계수기, 스택을 두어야하는 문제점이 있음
- 구현방법: 해시테이블 + 이중 연결 리스트
    - 해시 테이블은 이중 연결 리스트에서 빠르게 찾을 수 있도록 쓰고
    - 이중 연결 리스트는 한정된 메모리를 나타냄

#### 4. NUR(Not Used Recently)
- LRU에서 발전한 알고리즘으로 일명 clock 알고리즘이라함
- 0과 1을 가진 비트를 둠
    - 1: 최근에 참조 됨, 0: 참조되지 않음
- `시계 방향으로 돌면서 0(참조되지 않음)을 찾고` 0을 찾은 순간 해당 프로세스를 교체하고, 해당 부분을 1로 바꾸는 알고리즘

#### 5. LFU(Least Frequently Used)
- `가장 참조 횟수가 적은` 페이지 교체. 즉, 많이 사용되지 않은 것을 교체
