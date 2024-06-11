# basic-network-2024
2024년 IoT개발자과정 네트워크 통신 리포지토리


## 1일차

- 리눅스
    - 파일 입출력(LOW-level File Access)
    - 파일 디스크립터(File Descriptor)
    - 파일 열기 (open)
        - path : 파일 이름을 나타내는 문자열의 주소 값 전달.
        - flag : 파일의 오픈 모드 정보 전달
    
    - 파일 닫기 (close)
        - fd : 닫고자 하는 파일 또는 소켓의 파일 디스크립터 전달.

    - 파일에 데이터 쓰기 (writh)
        - fd : 데이터 전송대상을 나타내는 파일 디스크립터 전달.
        - buf : 전송할 데이터가 저장된 버퍼의 주소 값 전달.
        - nbytes : 전송할 데이터의 파이트 수 전달.

    - 파일에 저장된 데이터 읽기(read)
        - fd : 데이터 수신대상을 나타내는 파일 디스크립터 전달
        - buf : 수신한 데이터를 저장할 버퍼의 주소 값 전달
        - nbytes : 수신할 최대 바이트 수 전달

- 프로토콜
    - 소켓의 생성
        - domain : 소켓이 사용할 프로토콜 체계(Protocol Family)정보 전달
        - type : 소켓의 데이터 전송방식에 대한 정보 전달.
        - protocol : 두컴퓨터간 통신에 사용되는 프로토콜 정보 전달

    - 프로토콜 체계
        - PF-INET : IPv4 인터넷프로토콜 체계
        - PF_INET6 : IPV6 인터넷 프로토콜 체계
        - PF_LOCAL : 로컬 통신을 위한 UNIX 프로토콜 체계
        - PF_PACKET : Low Level 소켓을 위한 프로토콜 체계
        - PF_IPX : IPX 노벨 프로토콜 체계

    - 소켓의 타입
        - 연결지향형 소켓(TCP) 
            - 중간에 데이터가 소멸되지 않는다.
            - 전송 순서대로 데이터가 수신된다.
            - 전송되는 데이터의 경계(Boundary)가 존재하지 않는다. 

        - 비 연결지향형 소켓 (UDP)
            - 전송된 순서에 상관없이 가장 빠르게 전송을 지향한다.
            - 전송된 데이터는 손실의 우려가 있고, 파손의 우려가 있다.
            - 전송되는 데이터의 경계(Boundary)가 존재한다.
            - 한번에 전송할 수 있는 데이터의 크기가 제한된다.

    - IP주소와 PORT번호
        - 인터넷 주소(Inernet Address)
            - IPv4(Internet Protocol version4)  4바이트 주소체계
            - IPv6(Internet Protocol version6)  16바이트 주소체계

    - 클래스 별 네트워크 주소와 호스트 주소의 경계
        - 클래스 A의 첫 번째 바이트 범위 : 0이상 127이하  /  첫 번째 빝트는 항상 0으로 시작
        - 클래스 B의 첫 번째 바이트 범위 : 128이상 191이하  /  첫 두 비트는 항상 10으로 시작
        - 클래스 C의 첫 번째 바이트 범위 : 192이상 233이하  /  첫 세비트는 항상 110으로 시작

    - 주소정보의 표현
        - POSIX(Portable Operating System Interface)은 이식 가능한 운영 체제 인터페이스의 약자로,
          UNIX 운영 체제 호환성을 위한 표준 규격을 정의한 것입니다.
          POSIX는 다양한 운영 체제 간의 호환성을 보장하기 위해 IEEE가 개발했으며,
          다음과 같은 주요 구성 요소를 포함합니다:

        - 파일 시스템: 파일 및 디렉터리 조작에 관한 표준 인터페이스.
        - 프로세스 관리: 프로세스 생성, 제어, 종료 등에 관한 인터페이스.
        - 스레드: 멀티스레딩을 지원하기 위한 표준 인터페이스.
        - 입출력(I/O): 파일, 장치, 네트워크 소켓 등의 입출력 처리를 위한 표준 규격.
        - 유틸리티: 쉘 및 명령어 인터프리터, 시스템 명령어 및 유틸리티의 표준.

    - 구조체 sockaddr_in 멤버
        - 멤버 sin_family
            - AF_INET : IPv4 인터넷 프로토콜에 적용하는 주소체계
            - AF_INET6 : IPv6 인터넷 프로토콜에 적용하는 주소체계
            - AF_LOCAL : 로컬 통신을 위한 유닉스 프로토콜의 주소체계

        - 멤버 sin_port : 16비트 PORT번호 저장 / PORT 번호를 저장한다는 사실 보다 네트워크 바이트 
                          순서로 저장해야 한다는 사실이 더 중요하다.
        - 멤버 sin_addr : 32비트 IP주소정보 저장 / 네트워크 바이트 순서대로 저장하며 구조체 
                          in_addr도 함께 살펴봐야 한다.
        - 멤버 sin_zero : 구조체(struct sockaddr_in)에서 사용되는 멤버입니다. 이 멤버는 구조체의 
                          크기를 다른 주소 구조체와 맞추기 위해 사용되는 패딩 공간으로,
                          실제 데이터는 포함되지 않고 sin_zero는 보통 8바이트 크기의 배열임, 반드시 0으로 채워야 함.

    - 바이트 순서 변환 메모
        - 빅 엔디안 (Big Endian) : 높은 자리 바이트가 낮은 메모리 주소에 저장
            - 예: 0x12345678 저장
        ```
           주소:  |  값:
           -------|-------
             0    |  0x12
             1    |  0x34
             2    |  0x56
             3    |  0x78
        ```

        - 리틀 엔디안 (Little Endian) : 낮은 자리 바이트가 낮은 메모리 주소에 저장
            - 예: 0x12345678 저장

        ```
            주소:  |  값:
            -------|-------
              0    |  0x78
              1    |  0x56
              2    |  0x34
              3    |  0x12
        ```

        - 빅 엔디안 → 리틀 엔디안 : 0x12345678 -> 0x78563412
        - 리틀 엔디안 → 빅 엔디안 : 0x78563412 -> 0x12345678
        - 리틀 엔디안에서 빅 엔디안
        ```
            uint32_t little_to_big(uint32_t val) {
                return ((val & 0x000000FF) << 24) |
                       ((val & 0x0000FF00) << 8)  |
                       ((val & 0x00FF0000) >> 8)  |
                       ((val & 0xFF000000) >> 24);
            }
        ```
        - 빅 엔디안에서 리틀 엔디안
        ```
            uint32_t big_to_little(uint32_t val) {
                return ((val & 0x000000FF) << 24) |
                       ((val & 0x0000FF00) << 8)  |
                       ((val & 0x00FF0000) >> 8)  |
                       ((val & 0xFF000000) >> 24);
            }
        ```