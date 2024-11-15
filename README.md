# SuminNetCompose 🐱

## 📜 프로젝트 개요
이 프로젝트는 Docker Compose와 FRRouting을 사용하여 OSPF 라우팅 기반의 네트워크 환경을 자동으로 구성하는 스터디 목적으로 만들어졌습니다.

---

## 🌐 네트워크 구성
### 라우터 간 연결
- `10.10.0.0/16`: **Router1 ↔ Router2**
- `11.11.0.0/16`: **Router2 ↔ Router3**

### 서브넷 구성
- `192.168.40.0/24`: **Router1 ↔ Ubuntu1**
- `192.168.50.0/24`: **Router2 ↔ Ubuntu2**
- `192.168.60.0/24`: **Router3 ↔ Ubuntu3**

---

## 📊 네트워크 다이어그램
![network_compose drawio](https://github.com/user-attachments/assets/fd09047a-a930-4819-b2f1-c8b7581ca57f)

---

## ⚙️ 주요 기능
- Docker Compose를 사용해 **네트워크 환경 자동 구성**
- FRR 라우터에서 **OSPF 동적 라우팅 설정**
- Ubuntu 컨테이너 간 **Ping 및 Traceroute 테스트**

---

## 📦 요구 사항
- **Docker**: v20.10 이상
- **Docker Compose**: v1.29 이상

---

## 🚀 설치 및 실행
1. **레포지토리 클론**:
   ```bash
   git clone https://github.com/username/SuminNetCompose.git
   cd SuminNetCompose
2. **Docker Compose 실행**:
   ```bash
   docker-compose up --build
3. **테스트 방법**:
   ```bash
    # Ubuntu -> router 통신 확인
    docker exec -it ubuntu1 ping 192.168.40.3
    docker exec -it ubuntu2 ping 192.168.50.3
    docker exec -it ubuntu3 ping 192.168.60.3
    # Ubuntu간 통신 확인
    docker exec -it ubuntu1 ping 192.168.50.2 # Ubuntu1 -> Ubuntu2
    docker exec -it ubuntu2 ping 192.168.60.2 # Ubuntu2 -> Ubuntu3
    docker exec -it ubuntu3 ping 192.168.40.2 # Ubuntu3 -> Ubuntu1
    ```
- 만약 제대로 통신이 되지 않는다면 라우터가 아직 학습중인겁니다. 조금만 기다린 후 다시 ping 보내주세요😃

### 테스트 명령어
1. **컨테이너 네트워크 확인**
   ```bash
   docker network inspect network-compose subnet_192_168_40
   docker network inspect network-compose_subnet_192_168_50
   docker network inspect network-compose_subnet_192_168_60
   ```
2. **Gatewy 확인**
   ```bash
   # ubnutu1 컨테이너 안에 들어가는 명령어 
   docker exec -it ubuntu1 /bin/bash 
   ip route
   ```
   - 다른 우분투도 동일한 명령어로 번호만 변경해서 확인하시면 됩니다~!

4. **OSPF Neighbor 확인 & 라우팅 테이블 확인**
   ```bash
   #vtysh = FRRouter에서 라우터 설정하는 쉘
   docker exec -it router1 vtysh 
   show ip ospf neighbor
   ----------------------------------------------------------------
   show ip route #라우팅 테이블 확인 명령어
   ```
   - 라우터 번호만 변경해서 다른 라우터도 확인해보시면 좋습니당~
5. **Traceroute 테스트**
   ```bash
   # Ubuntu1 -> Ubuntu3 까지의 경로
   docker exec -it ubuntu1 traceroute 192.168.60.2
   
   # Ubutnu3 -> Ubuntu3 까지의 경로
   docker exec -it ubuntu3 traceroute 192.168.40.2 
   ```
---

## 🗺️디렉터리 구조
```
├── configs/
│   ├── router1/
│   │   └── frr.conf
│   ├── router2/
│   │   └── frr.conf
│   └── router3/
│       └── frr.conf
├── Dockerfile.ubuntu1
├── Dockerfile.ubuntu2
├── Dockerfile.ubuntu3
├── Dockerfile.router
├── docker-compose.yml
├── README.md
└── LICENSE
```
---

## 🛠️ 라이선스
이 프로젝트는 MIT 라이선스를 따릅니다. 자유롭게 사용, 수정, 재배포가 가능하며, 자세한 내용은 [LICENSE](./LICENSE) 파일을 참고하세요.


