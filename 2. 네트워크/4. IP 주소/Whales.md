## Section 2.4 IP 주소

### 2.4.1 ARP(Address Resolution Protocol)

> 컴퓨터와 컴퓨터 간의 통신 - IP 주소에서 ARP를 통해 MAC 주소를 찾아 MAC 주소를 기반으로 통신
> 
- IP와 MAC 주소의 다리 역할을 하는 프로토콜
- 가상 주소인 IP 주소를 실제 주소인 MAC 주소로 변환
↔ RARP: MAC 주소를 IP 주소로 변환
    
    장치 A가 ARP Request 브로드캐스트를 보내어 IP 주소에 해당하는 MAC 주소를 찾는다.
    해당 주소에 맞는 장치 B가 `ARP reply 유니캐스트`를 통해 MAC 주소를 반환하는 과정을 거쳐
    IP에 맞는 MAC를 찾는다.
    

<aside>
💡 브로드캐스트
: 송신 호스트가 전송한 데이터가 네트워크에 연결된 모든 호스트에 전송되는 방식

💡 유니캐스트: 고유 주소로 식별된 하나의 네트워크 목적지에 1:1 로 데이터를 전송하는 방식

</aside>

### 2.4.2 홉바이홉(Hop by hop) 통신

- hop: 건너뛰는 모습
- 통신망에서 각 패킷이 여러 개의 라우터를 건너가는 모습을 비유적으로.
- IP 주소를 통해 통신하는 과정
- 통신 장치에 있는 `라우팅 테이블`의 IP를 통해 시작 주소부터 시작하여 다음 IP로 계속해서 이동하는 `라우팅`과정을 거쳐 패킷이 최종 목적지까지 도달하는 통신
* 라우팅: IP 주소를 찾아가는 과정
    
    - 라우팅 테이블(Routing table)
    송신지에서 수신지까지 도달하기 위해 사용.
    라우터에 들어가있는 목적지 정보들과 가기 위한 방법이 들어 있는 리스트.
    게이트웨이와 모든 목적지에 대해 해당 목적지에 도달하기 위해 거쳐야할 다음 라우터의 정보를 가지고 있다
    - 게이트웨이(Gateway)
    서로 다른 통신망, 프로토콜을 사용하는 네트워크 간의 통신을 가능하게 하는 관문 역할의
    컴퓨터나 소프트웨어
    라우팅 테이블을 통해 볼 수 있다.
    윈도우 명령 프롬프트에서 `netstat -r` 명령어를 실행하여 확인 가능
        
### 2.4.3 IP 주소 체계

- IPv4
    - 32비트를 8비트 단위로 점을 찍어 표기
    - 123.45.67.89 같은 방식
- IPv6
    - 64비트를 16비트 단위로 점을 찍어 표기
    - 2001:db8::ff00:42:8329 같은 방식

- 클래스 기반 할당 방식
    - 클래스 A, B, C - 일대일 통신
    클래스 D - 멀티캐스트 통신
    클래스 E - 예비용
    - 사용하는 주소보다 버리는 주소가 많다는 단점
    → DHCP, IPv6, NAT 등장
- DHCP(Dynamic Host Configuration Protocol)
    - IP 주소 및 기타 통신 매개변수를 자동으로 할당하기 위한 네트워크 관리 프로토콜
    - 인터넷에 접속할 때마다 자동으로 IP 주소 할당(네트워크 장치의 IP 주소를 수동으로 설정할 필요 X)
    - 많은 라우터와 게이트웨이 장비에 DHCP 기능 유
    → 대부분의 가정용 네트워크에서 IP 주소를 할당
- NAT(Network Address Translation)
    - 패킷이 라우팅 장치를 통해 전송되는 동안 패킷의 IP 주소 정보를 수정하여 IP 주소를 다른 주소로 매핑
    - 공인 IP와 사설 IP로 나눠서 많은 주소를 처리(← IPv4 주소 체계만으로는 많은 주소들 감당 불가)
    - NAT를 가능하게 하는 소프트웨어 - ICS, RRAS, Netfilter
    - 공유기에 NAT 기능 탑재 → 여러 대의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷 접속 가능
    - NAT를 이용한 보안 → 내부 네트워크 IP 주소와 외부에 드러나는 IP 주소를 다르게 유지 가능
    - 단점 → 실제로 접속하는 호스트 숫자에 따라 접속 속도가 느려질 수 있다.

### 2.4.4 IP 주소를 이용한 위치 정보

- [mylocation 사이트 링크](https://mylocation.co.kr/)
