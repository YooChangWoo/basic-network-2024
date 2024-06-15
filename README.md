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
        - 비 연결지향형 소켓 (UDP)

    - 소켓의 함수
        - socket(): 소켓 생성.
        - bind(): 소켓에 IP 주소와 포트 번호를 할당.
        - listen(): 소켓을 수신 대기 상태로 설정 (서버용).
        - accept(): 연결 요청을 수락하고 새로운 소켓을 생성 (서버용).
        - connect(): 서버에 연결 요청 (클라이언트용).
        - send(), recv(): 데이터 송수신.
        - close(): 소켓 종료.

    - 주소 체계
        - IPv4(Internet Protocol version4)  4바이트 주소체계
        - IPv6(Internet Protocol version6)  16바이트 주소체계

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


## 2일차 
- TCP/IP 프로토콜 스택 : TCP/IP 프로토콜 스택은 일반적으로 네트워크 계층 모델을 따르며, 여러 계층으로 구성됩니다.
                        가장 일반적으로 사용되는 모델은 OSI 모델과 유사하지만 약간 다를 수 있습니다. 주요 계층은 다음과 같습니다:
    - LINK 계층 (또는 네트워크 인터페이스 계층)        
    - IP 계층 (인터넷 계층)
    - TCP/UDP 계층 (전송 계층)
    - APPLICATION 계층

- TCP 서버에서의 기본적인 함수호출 순서
    - 1. 소켓 생성 (Socket Creation)
    - 2. 주소와 포트 바인딩 (Binding Address and Port)
    - 3. 연결 수신 대기 (Listening for Connections)
    - 4. 연결 수락 (Accepting Connections)
    - 5. 데이터 통신 (Data Communication)
    - 6. 연결 종료 (Closing Connection)

- Iterative 에코 서버, 에코 클라이언트
    - 에코 서버 및 클라이언트는 클라이언트가 서버로 메시지를 보내면 서버가 동일한 메시지를 클라이언트에게 다시 보내는 간단한 통신 모델

- TCP의 내부 동작원리
    - 1. 연결 설정 (3-way 핸드셰이크)
        - SYN: 클라이언트가 서버에 연결 요청 (SYN 패킷 전송).
        - SYN-ACK: 서버가 클라이언트의 요청을 수락하고 응답 (SYN-ACK 패킷 전송).
        - ACK: 클라이언트가 서버의 응답을 확인하고 연결 설정 완료 (ACK 패킷 전송).
    - 2. 데이터 전송
        - 데이터 분할: 애플리케이션 데이터를 작은 세그먼트로 분할.
        - 순서 번호: 각 세그먼트에 순서 번호를 부여하여 순서 보장.
        - 확인 응답 (ACK): 수신한 세그먼트에 대해 수신 확인 응답을 전송.
        - 재전송: 확인 응답을 받지 못한 세그먼트는 재전송하여 신뢰성 보장.
    - 3. 연결 종료 (4-way 핸드셰이크)
        - FIN: 클라이언트가 연결 종료 요청 (FIN 패킷 전송).
        - ACK: 서버가 클라이언트의 요청을 확인 (ACK 패킷 전송).
        - FIN: 서버도 연결 종료 요청 (FIN 패킷 전송).
        - ACK: 클라이언트가 서버의 요청을 확인하고 연결 종료 (ACK 패킷 전송).


- UDP 소켓의 특성
    - 비연결성 : UDP는 연결 설정 과정이 없이 데이터를 주고받습니다. 따라서 TCP와는 달리 연결을 설정하고 해제하는 과정이 없습니다.
    - 신뢰성 없음 : UDP는 데이터 전송 시 신뢰성을 보장하지 않습니다. 데이터 전송 중 손실이나 손상이 발생할 수 있으며, 이를 처리하기 위한 재전송이나 흐름 제어 기능이 없습니다.
    - 흐름 제어 없음 : TCP와 달리 UDP는 흐름 제어 기능이 없습니다. 따라서 데이터를 수신할 수 있는 속도를 조절할 수 없으며, 수신 측의 버퍼 오버플로우를 방지할 수 있는 방법이 없습니다.
    - 순서 보장 없음 : UDP는 데이터를 순서대로 전송하지 않습니다. 따라서 데이터가 도착하는 순서가 전송한 순서와 일치하지 않을 수 있습니다.
    - 작은 헤더 오버헤드 : UDP 헤더는 TCP보다 훨씬 간단합니다. 이는 추가적인 오버헤드가 적기 때문에 데이터 전송에 있어서 더 효율적입니다.
    - 빠른 전송 속도 : TCP보다 빠른 전송 속도를 가집니다. 연결 설정 및 흐름 제어 기능이 없기 때문에 데이터 전송이 더 빠릅니다.

## 3일차

- connected UDP 소켓, unconnected UDP 소켓
    - sendto 함수 호출을 통한 데이터 전송
        - 1단계 UDP 소켓에 목적지의 IP와 PORT번호 등록
        - 2단계 데이터 전송
        - 3단계 UDP 소켓에 등록된 목적지 정보 삭제

    - connected UDP 소켓 생성
        ```
        sock=socket(PF_INET, SOCK_DGRAM, 0);
        memset(&adr, 0, sizeof(adr));
        adr.sin_family = AF_INET;
        adr.sin_addr.s_addr = ......;
        adr.sin_port = ........;
        connect(sock, (struct sockaddr*)&adr, sizeof(adr))
        ```

- 소켓의 연결종료
    - 소켓과 스트림(Stream)
    - shutdown 함수
        - SHUT_RD  :  입력 스트림 종료
        - SHUT_WR  :  출력 스트림 종료
        - SHUT_RDWR  :  입출력 스트림 종료
    - Half-close

- 도메인 이름과 인터넷 주소
    - TCP 기반의 Half-close
        - Domain Name System
            - DNS 서버
            - 도메인 이름을 이용해서 IP주소 얻어오기
                - h_name
                - h_aliases
                - h_addrtype
                - h_length
                - h_addr_list
        - IP주소를 이용해서 도메인 정보 얻어오기
            - gethostbyname


- 소켓의 다양한 옵션
    - 소켓의 옵션과 입출력 버퍼의 크기
        - 소켓의 다양한 옵션
        - getsockopt & setsockopt
        - SO_SNDBUF & SO_RCVBUF

    - SO_REUSEADDR
        - 주소할당 에러(Binding Error) 발생
        - Time-wait 상태
        - 주소의 재할당

    - TCP_NODELAY
        - Nagle 알고리즘
        - Nagle 알고리즘의 중단

- 멀티프로세스 기반의 서버구현
    - 프로세서의 이해와 활용
        - 다중접속 서버의 구현방법들
            - 멀티프로세스 기반 서버 : 다수의 프로세스를 생성하는 방식으로 서비스 제공
            - 멀티플렉싱 기반 서버 : 입출력 대상을 묶어서 관리하는 방식으로 서비스 제공
            - 멀티쓰레딩 기반 서버 : 클라이언트의 수만큼 쓰레드를 생성하는 방식으로 서비스 제공

        - fock 함수
            - 부모 프로세스 : fork 함수의 반환 값은 자식 프로세스의 ID
            - 자식 프로세스 : FORK 함수의 반환 값은 0

- 프로세스 & 좀비(Zombie)프로세스
    - 좀비(Zombie)프로세스 생성이유
        - 인자를 전달하면서 exit를 호출하는 경우
        - main 함수에서 return문을 싱행하면서 값을 반환하는 경우
       


## 4일차
- 좀비 프로세스의 소멸 : wait 함수의 사용
    - WIFEXITED : 자식 프로세스가 정상 종료한 경우 '참(true)'를 반환 한다.
    - WEXITSTATUS : 자식 프로세서의 전달 값을 반환한다.

- 시그널 핸들링
    - SIGALRM : alarm 함수호출을 통해서 등록된 시간이 된 상황
    - SIGINT : CTRL + C가 입력된 상황
    - SIGCHLD : 자식 프로세스가 종료된 상황
        - signal(SIGCHLD, mychild), signal(SIGALRM, timeout);, signal(SIGINT, keycontorol);
    - sigaction
        - sa_handler, sa_mask, sa_flags

- 멀티태스킹 기반의 다중접속 서버
    - 프로세스 기반의 다중접속 서버의 구현 모델(fork)
        - 1. 에코서버(부모 프로세스)는 accept 함수호출을 통해서 연결요청을 수락한다.
        - 2. 이때 얻게 되는 소켓의 파일 디스크리버를 자식프로세스를 생성해서 넘겨준다.
        - 3. 자식 프로세스는 전달받은 파일 디스크립터를 바탕으로 서비스를 제공 한다.

- 프로세스간 통신
    - 파일을 통한 통신: 프로세스는 파일을 통해 데이터를 주고받을 수 있습니다. 이는 많은 운영 체제에서 지원되는 간단하고 효율적인 방법입니다.
    - 파이프: 단방향 또는 양방향으로 데이터를 전송할 수 있는 파이프(pipe)를 사용하여 프로세스 간에 통신할 수 있습니다. 이는 리눅스 및 유닉스 시스템에서 널리 사용됩니다.
    - 소켓: 네트워크를 통해 데이터를 전송하는 데 사용되는 소켓은 프로세스 간 통신을 위한 강력한 방법입니다. TCP/IP 소켓은 인터넷을 통한 통신에 주로 사용됩니다.
    - 메시지 큐: 메시지 큐를 사용하여 프로세스 간에 데이터를 전송할 수 있습니다. 이는 다양한 운영 체제에서 지원되며, 다른 프로세스에 메시지를 보낼 수 있습니다.
    - 공유 메모리: 공유 메모리를 사용하면 여러 프로세스가 동일한 메모리 공간에 접근하여 데이터를 공유할 수 있습니다. 이는 매우 빠른 통신을 제공하지만 조심스럽게 사용해야 합니다.

- IO 멀티플렉싱
    - IO 멀티플렉싱은 단일 프로세스가 여러 개의 입출력 작업을 동시에 처리할 수 있도록 하는 기술이고,
      주로 네트워크 프로그래밍이나 파일 입출력에서 사용됩니다. 다음은 IO 멀티플렉싱의 주요 개념과 장점을 설명한 것입니다:

        - 단일 스레드 환경: IO 멀티플렉싱은 단일 스레드에서 여러 개의 IO 작업을 처리할 수 있도록 합니다.
          이는 멀티 스레드를 사용하는 것보다 적은 자원을 사용하고 더 효율적으로 작업을 수행할 수 있습니다.

        - 이벤트 기반 처리: IO 멀티플렉싱은 주로 이벤트 기반의 프로그래밍 모델과 함께 사용됩니다. 
          입출력 작업이 완료되었을 때 발생하는 이벤트를 감지하고 처리함으로써 비동기적으로 작업을 수행할 수 있습니다.

        - select(), poll(), epoll(): 이러한 함수들은 다양한 플랫폼에서 IO 멀티플렉싱을 구현하는 데 사용됩니다. 
          select는 가장 오래된 방법으로, poll과 epoll은 보다 효율적인 대안으로 각각 다른 운영 체제에서 사용됩니다.

        - 비동기 소켓 프로그래밍: 네트워크 프로그래밍에서 IO 멀티플렉싱은 비동기 소켓을 통해 여러 클라이언트와 동시에 통신할 수 있도록 합니다.
          이를 통해 블로킹되지 않고 여러 연결을 처리할 수 있습니다.

        - 사용 사례: 대규모 웹 서버나 채팅 애플리케이션과 같이 동시에 많은 연결을 처리해야 하는 경우에 IO 멀티플렉싱은 매우 유용합니다.
          또한 파일 입출력 작업에서도 여러 파일을 동시에 처리할 때 유용하게 사용됩니다.

    

       
