# TCP/IP 4계층 모델과 OSI 7계층

이 둘은 인터넷상에서 컴퓨터들이 서로 정보를 주고받는 데 쓰이는 통신규약(`protocol`)들의 모음으로 `internet protocol suite`라고 부릅니다.  
`TCP/IP 4계층`과 `OSI 7계층`은 모두 `internet protocol suite`를 설명하기 위해 사용되는 모델입니다. 추상화 정도의 차이가 있을 뿐 같은 것을 설명하는 모델입니다.

![Application Layer](/res/Application_Layer.png "Application Layer")

## TCP/IP 4 계층

이 글에서는 `TCP/IP 4계층`을 중심으로 바라보고자 합니다. 먼저 조금 더 명확한 그림을 떠올리기 위해 각 추상화 레이어에 해당하는 예시부터 알아보겠습니다.

| TCP/IP 4 Layer             | example                             |
| :------------------------- | :---------------------------------- |
| **`Application Layer`**    | `FTP` / `HTTP` / `SSH` / SMTP / DNS |
| **`Transport Layer`**      | `TCP` / `UDP` / QUIC                |
| **`Internet Layer`**       | `IP` / ARP / ICMP                   |
| **`Network Access Layer`** | `이더넷`                            |

익숙한 프로토콜들도 몇몇 보입니다.

이렇게 추상화된 각각의 계층들은 서로 다른 계층이 영향을 받지 않도록 설계되었습니다. 예를 들어, `Transport Layer`에서 `TCP`를 `UDP`로 변경하더라도 `Application Layer`에 해당하는 웹 브라우저등을 변경할 필요가 없다는 뜻 입니다.

각각의 계층에 대해서 조금 더 자세히 알아보겠습니다.

## Application Layer

`FTP/HTTP/SSH` 등이 `Application Layer`에서 사용되는 대표적인 프로토콜입니다. 이 프로토콜들은 응용 프로그램에서 사용되어 파일 전송, 웹, 이메일 등 서비스를 실질적으로 사람들에게 제공하는 계층 입니다.

| Protocol | 설명 |
| :-- | :-- |
| `FTP` | 파일을 전송하는 데 사용되는 표준 통신 프로토콜 |
| `SSH` | 보안되지 않은 네트워크에서 네트워크 서비스를 안전하게 운영하기 위한 암호화 네트워크 포로토콜 |
| `HTTP` | World Wide Web을 위한 데이터 통신의 기초이자 웹 사이트를 이용하는 데 쓰이는 프로토콜 |
| `SMTP` | 전자 메일 전송을 위한 인터넷 표준 통신 프로토콜 |
| `DNS` | IP주소와 도메인 이름을 매핑해주는 서버 |

## Transport Layer

송신자와 수신자를 연결하는 통신 서비스를 제공하는 계층입니다. 연결 지향 데이터 스트림, 신뢰성, 흐름제어 등의 기능을 제공하며 어플리케이션과 인터넷 계층 사이의 데이터가 전달될 때 중계하는 역할을 맡습니다. 대표적인 프로토콜은 `TCP`와 `UDP`입니다.

## Internet Layer

이 계층은 장치로부터 받은 `네트워크 패킷`을 IP주소로 지정된 목적지로 전송하기 위해 사용되는 계층입니다. 대표적인 프로토콜은 `IP`가 있습니다. 이 계층은 패킷을 수신해야 할 상대의 주소를 지정하여 데이터를 전달하기만 합니다. 상대방이 제대로 받았는지에 대해 보장하지는 않는 비연결형적인 특징을 갖습니다.

## Network Access Layer

`Link Layer`라고도 불리는 계층으로 전선/광섬유/무선 등을 통해 실질적으로 데이터를 전달하며 장치간 신호를 주고받는 규칙을 정하는 계층입니다. `OSI 7 계층`에서는 이 계층이 `Physical Layer`와 `Data Link Layer`로 나뉘는 데, `Physical Layer`는 이름 그대로 유/무선 LAN등을 통해 0과 1로 이루어진 데이터를 보내는 계층을 의미하며, `Data Link Layer`는 이더넷 프레임을 통해 에러를 확인하고 흐름이나 접근을 제어하는 계층을 의미합니다.

> `WIFI`에 접속하는 행위는 `Network Access Layer`에서 이루어집니다.
