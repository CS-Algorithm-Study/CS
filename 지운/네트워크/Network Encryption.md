# Network Encryption


## 목차
1. 개념
2. 암호 보안 기술

---

## 1. 개념
- `암호화` : 네트워크로 전송되는 데이터의 보안을 위해 사용하는 방법 중 하나. 통상 `Key` 라는 비밀 값을 복잡한 알고리즘과 결합해 데이터를 암호화 또는 해독함
- `평문(Plain text)` : 입력되는 원본 문서
- `암호문(Cipher text)` : 평문을 암호화한 문서
- `암호화(Encryption)` : 평문을 암호문으로 변환하는 과정
- `복호화(Decryption)` : 암호문을 평문으로 변환하는 과정
- `키(Key)` : 암호화 및 복호화 시 필요한 값으로, 매우 큰 수로 구성
- `키 스페이스(Keyspace)` : 키 값의 범위

## 2. 암호 보안 기술
### 2-1. 양방향 암호화
- 암호화와 복호화를 통해 송수신 간 주고받는 메시지를 안전하게 암호화하는 과정
#### 1) 비밀키(대칭키) 암호(Secret key = Symmetric key)
![비밀키](https://k0102575.github.io/assets/image/2020-04-04-encryption/image1.png)
**개념**
- `암호키 = 복호키` 인 암호화 방식
- 두 키가 같으므로 암호화 및 복호화가 빠름
- 복수의 사용자 사용 시 키를 교환할 때의 보안 문제 (`키 배송 문제`) 발생
> `키 배송 문제`
: 송신 측에서 수신 측에 암호 키를 전달해야만 하는데, 키가 중간에 탈취되면 평문이 반드시 드러나게 됨.
결국 안전하게 평문을 전달하고자 만든 것이 암호문인데, 정작 키는 안전하게 전달할 방법이 없는 상황 발생

**종류**
1. 스트림 암호화
  - 비트 단위로 암호화
  - 속도가 빠르며 오류 전파 현상 X
  - 주로 오디오, 비디오 스트리밍에 사용
<br>![스트림 암호화](https://velog.velcdn.com/images%2Finyong_pang%2Fpost%2F66538238-a58e-40c8-9d37-f0bb8792dc44%2Fimage.png)
2. 블록 암호화
  - 블록 단위로 암호화
  - DES는 이전에 사용, AES는 현재 널리 사용
<br>![블록 암호화](https://velog.velcdn.com/images%2Finyong_pang%2Fpost%2F4a894f60-5d61-4eb8-b505-2714620be139%2Fimage.png)
#### 2) 공개키(비대칭키) 암호(Public key = Asymmetric key)
![공개키](https://k0102575.github.io/assets/image/2020-04-04-encryption/image2.png)
**개념**
- `암호키 ≠ 복호키` 인 암호화 방식
- 암호키는 `공개키`로, 누구나 이를 이용해 암호화 가능
- 복호키는 `개인키`로, 개인만 가짐
- `키 배송 문제`를 해결하기 위해 생겨난 방식
- 보안상 문제가 발생할 여지가 적으나(중간자 공격`MITM`에 취약) 암호화 및 복호화 속도가 느림, 다량의 자료에 적용하기 어려움
> `MITM(Man In The Middle Attack)`
: 해커가 중간에서 통신을 가로챈 뒤, 수신자에게 송신자인 척하고 송신자에게 수신자인 척하여 양쪽의 공개키 및 복호키를 얻어내는 기법

**종류**
1. 인수분해
  - `RSA` : 메시지 암호화, 복호화

2. 이산대수
  - `DSA` : 전자서명

3. 타원곡선(ECC)
  - 인수분해 방식(RSA)에 비해 짧은 길이의 키를 사용하면서 비슷한 수준의 안정성 제공
  - 비트코인, 이더리움 등에서 사용되는 알고리즘
> `ECDSA(Elliptic Curve Digital Signature Algorithm)`
: 전자서명(ECC 암호화 알고리즘을 전자서명에 사용한 것)
`ECDH(Elliptic Curve Diff-Hellman)`
: 키교환 알고리즘(자신의 Private Key와 상대방의 Public Key를 사용하여 공통된 Secret 키를 도출)
`ECIES(Elliptic Curve Integreated Encryption Scheme)`
: 통합 암호화 방식(Public Key로 암호화하고 Private Key로 복호화)

#### 3) 정리
![전체 표](https://velog.velcdn.com/images%2Finyong_pang%2Fpost%2F70aa687b-de53-44c4-982a-6e7bcd26e221%2Fimage.png)

### 2-2. 단방향 암호화
#### 1) 개념
- Hash를 이용한 암호화
  - 임의의 길이 메시지로부터 고정길이의 Hash 값 계산
  - Hash 값으로부터 메시지를 역산하는 과정 불가
- 평문의 암호화는 가능, **복호화는 불가능**
- 메시지가 다를 경우 Hash 값도 달라짐(무결성)
- 데이터의 프라이버시를 지키면서 진위여부를 확인할 때 사용 (예. 비밀번호 암호화)
- Hash 값은 크기와 알고리즘에 따라 암호문의 결과가 다름

#### 2) 한계 및 보완
- 한계
  - Rainbow Table Attack : 미리 계산한 Hash 값 테이블(Rainbow Table)을 이용해 암호화 값이 맞을 때까지 공격
  - Hash 함수의 특성 상 **처리속도가 빠르게 설계**되어 있어, **임의 문자열 다이제스트와 해킹할 다이제스트의 비교에 오랜 시간이 걸리지 않음**
- 보완
  - Salting : 실제 메시지에 추가로 랜덤 데이터를 더해 Hash 값 계산
  - Key Stretching : 단방향 Hash 값 계산 후 그 값을 여러번 Hash
  - 시간을 더 오래 끌 수 있을 뿐, 궁극적인 해결책이 되지는 못함
  
#### 3) MAC(Message Authentication Code)
- 임의 길이의 메시지와 `송수신자가 공유하는 키` 를 기반으로, 고정 길이의 출력값을 계산하는 함수를 활용
- 마찬가지로 메시지의 무결성 확인 및 인증을 위해 사용
![MAC](https://velog.velcdn.com/images%2Fkdh10806%2Fpost%2F8035f009-b503-41ec-a2e7-f95a91968679%2F1322px-MAC.svg.png)
- 송신자 : 메시지와 키를 이용해 **MAC 값을** 구한 뒤 그 값을 **메시지에 끼워 보냄**
- 수신자 : 받은 메시지와 본인의 키를 이용해 **MAC 값을** 구하여 송신자가 보낸 MAC 값과 일치하는지 **비교**
