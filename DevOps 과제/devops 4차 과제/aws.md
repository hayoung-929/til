## 클라우딩
# AWS (Amazon Web Service)
<details>
<summary> 클라우드 컴퓨팅 (cloud computing)</summary>

- 인터넷 기반의 컴퓨팅으로 인터넷 상의 가상화 된 서버에 프로그램을 두고 필요 할때마다 컴퓨터나 스마트폰 등에 불러와 사용하는 서비스
- 인터넷만 연결된다면 언제 어디서든 저장한 데이터를 가져올 수 있다.
1. **IaaS (Infrastructure as a Service) 서비스형 인프라**
   - 클라우드에서 호스팅되는 컴퓨팅, 스토리지,네트워킹에 대한 주문형 액세스를 제공하는 클라우드 컴튜팅은 직접 설치,관리
   - ex) AWS EC2, Google Compute Engine 등
2. **PaaS (Platform as a Service) 서비스형 플랫폼**
   - 애플리케이션을 개발,실행,유지 관리 및 관리하기 위한 완전한 온디맨드 클라우드 플랫폼을 제공하는 클라우드 컴퓨팅 모델
    - 운영체제,런타임,미들웨어 전체 제공
    - 코드만 올리고 인프라 관리 필요 X
    - ex) AWS Elastic Beanstalk Google App Engine 등
3. **SaaS (Software as a Service)**
    - 인프라, 플랫폼, 앱을 서비스로 제공
    - 관리 X
    - ex) Gmail, Google Docs, Notion 등
4.  각각의 장단점
   - **IaaS**
      - 장점 : 내 마음대로 서버 설정 가능, OS부터 전부 커스터마이즈 가능, 비용 최적화, 어떤 환경과 언어 가능
      - 단점 : 직접 관리 할것이 많다, 보안패치 업데이트 다 직접, 운영 전문 인력 필요
   - **PaaS**
     - 장점 : 인프라 신경 X, 배포 간단, 자동 스케일링
     - 단점 : 플랫폼에 종속, 세부 설정 자유 낮음, 비용 증가
 - **SaaS**
   - 장점 : 설치 없이 바로 사용 가능, 관리 X, 어디서든 접속 가능, 자동 업데이트
   - 단점 : 커스터마이징 X, 데이터가 외부에 있음, 비용 더욱 증가
<details>
<summary>보충 개념</summary>

1. **온디맨드 (On-Demand)**
- 필요할 때만 자원을 요청하고 **쓴 만큼만 비용 지불**
- 반대 : 온프레미스 (고정지출) 
2. **가상화 (Virtualization)**
- 물리 서버 한대를 **소프트웨어적으로 여러 대로 분리**하는 기술
- **하이퍼바이저**가 자원 분배 담당
  - **type 1** 하드웨어 위에 직접 설치
  - **type 2** OS위에 설치
- 클라우드 온디맨드를 가능하게 하는 핵심 기술
3. **컨테이너** vs **가상머신**

    | | VM | 컨테이너 |
    |---|---|---|
    | 가상화 단위 | OS 전체 | 앱 환경만 |
    | 무게 | 무거움 | 가벼움 |
    | 부팅 속도 | 느림 | 빠름 |
    | 대표 도구 | VirtualBox, VMware | Docker |
- DevOps는 **Docker** & **Kubernetes**
4. **미들웨어 (Middleware)**
- 앱과 OS 사이에서 **중간 다리**
- 앱이 DB 연결, 네트워크 통신, 인증을 쉽게 하도록 도와줌
5. **런타임 (runtime)**
- 코드가 **실제로 실행되기 위한 환경**
- Python → CPython 인터프리터
- Java → JVM
- JavaScript → Node.js
- PaaS에서는 이걸 **플랫폼이 미리 설치**해둠
6. **스케일링 (Scaling)**
- **스케일 업(수직):** : 서버 한대의 CPU, RAM을 올림(한계 있음)
- **스케일 아웃(수평):** 서버 대수를 늘림 (클라우드의 핵심 방식)
- **오토 스케일링:** 트래픽에 따라 자동으로 서버 수 조절
  - DevOps에서 모니터링 + 오토스케일링 설정은 핵심 업무
7. **프로비저닝 (Provisioning)**
-  서버 자원을 **준비하고 설정하는 과정**
-  수동으로 하면 시간이 오래 걸리고 실수가 있음
8. DevOps에서 클라우드 흐름
```
    1. 코드 작성
    2. Git Push
    3. CI/CD 파이프라인 트리거(GItHub Action 등)
    4. 자동 빌드 - 자동 테스트
    5. Docker 이미지 생성
    6. 클라우드에 배포
    7. 모니터링 + 오토 스케일링
```
</details>
</details>

## AWS 개념
<details>
<summary>AWS의 의미</summary>

- **AWS (Amazon Web Service)**
- **서버 네트워크 등 인프라 전체를 빌려주는 서비스**
- 다양한 서비스 제공 (200개 이상)
- ex) EC2, Lambda, S3 등

1. **EC2**
- (Elastic Compute Cloud)
- AWS에서 빌려쓰는 가상 서버
- 운영체제 직접 설치, 설정
- 사용한 만큼 비용 지불
- IaaS
2. **Lambda**
- 서버 없이 함수 단위로 실행되는 서비스
- 요청이 들어올 때만 실행, 끝나면 사라짐
- 서버 관리X, 실행된 횟수만큼 비용 지불
- Serverless
- **Serverless**란?
  - 개발자가 서버 인프라를 직접 관리하지 않고 코드만 작성하여 배포하는 클라우드 네이티브 개발 모델 (FaaS, BaaS)
3. **S3**
- (Simple Storage Service)
- 파일을 저장하는 클라우드 창고
- 이미지, 동영상, 로그파일, 백업 등 어떤 파일이든 저장 가능
- 용량 제한 X, 저장한 만큼 비용 지불
</details>

<details>
<summary>AWS IAM</summary>

1. **IAM (Identity and Access Management)**
- 개념
  - AWS 리소스에 대한 접근 권한을 관리하는 서비스
  - "누가, 어떤 서비스에, 어떤 행동을 할 수 있는가"를 정의
- 구성요소
  - **User** : AWS 계정 안의 개별 사용자
  - **Group** : 여러 User를 묶어서 권한을 일괄 적용
  - **Role** : EC2, Lambda 같은 AWS 서비스에 부여하는 권한
  - **Policy** : 허용/거부할 행동을 정의한 JSON 문서, User/Group/Role에 붙여서 사용
- 특징
  - Root 계정은 모든 권한을 가지므로 일상적인 작업엔 사용 X
  - 최소 권한 원칙 : 필요한 권한만 부여하는 것이 보안 기본
  - MFA(다중 인증) 설정 권장
</details>

<details>
<summary>AWS 인프라 환경</summary>

1. **Region**
- AWS 데이터센터가 물리적으로 모여있는 지역 단위
- 리전끼리는 완전 독립
- 선택 기준
  - 사용자와 물리적으로 가까울 수록 응답 빠름
  - 법적 규제
  - 서비스 가격 (요금이 리전마다 다름)
2. **Availability Zone**
- 리전 안에 있는 실제 데이터센터 단위
- 하나의 리전에 최소 3개 AZ있음
- AZ끼리는 물리적으로 수십km 떨어짐
- 전용 광케이블로 연결되있어서 인터넷 빠름
3. **Edge Location**
- 리전/AZ와 완전히 별개로 전 세계에 퍼져 있는 캐시 서버
- 주로 CloudFront가 여기서 동작
- **CloudFront**란?
  - AWS에서 제공되는 고속 콘텐츠 전송 네트워크 서비스
4. **VPC**
- (Virtual Private Cloud)
- AWS 안에서 나만의 격리된 네트워크 공간
- 외부에서 함부로 접근 불가
- 리전 단위로 생성
5. **서브넷**
- VPC를 용도별로 쪼갠 구역
- **Public Subnet** : 인터넷 연결 가능 -> 웹 서버, 로드 밸런서
- **Private Subnet** : 인터넷 차단 -> DB, 내부 API
</details>
<details>
<summary>AWS Storage</summary>

- AWS에서는 물리적 하드디스크 대신 가상화된 스토리지 제공

1. **DAS**
- (Direct Attached Storage)
- 개념
  - 저장장치를 서버에 직접 연결하는 방식
  - 별도의 네트워크 없이 케이블로 바로 연결
  - **해당 서버만 접근 가능**
- 특징
  - **속도 빠름**
  - 확장성이 낮음
  - 서버가 꺼지면 데이터 접근 자체가 불가능
- AWS 해당 서비스 : **인스턴스 스토어**
  - EC2에 물리적으로 붙어 있는 임시 저장소
  - EC2 종료시 데이터 영구 삭제
  - 임시 캐시, 버퍼 용도로 사용
2. **NAS**
- (Network Attached Storage)
- 개념
  - 네트워크를 통해 여러 서버가 동시에 접근할 수 있는 저장장치
  - 파일 단위로 데이터를 주고 받음
  - TCP/IP 프로토콜 사용
- 특징
  - 여러 서버가 동시에 같은 파일 접근 가능
  - DAS보다 속도 느림
  - 확장성 높음
- AWS 해당 서비스 : **EFS**
  - 여러 EC2가 동시에 마운트해서 사용 가능
  - 용량 자동 확장/축소
  - Linux 기반 EC2에서 사용
3. **SAN**
- (Storage Area Network)
- 개념
  - 스토리지만을 위한 전용 고속 네트워크를 구성하는 방식
  - 블록 단위로 데이터를 주고 받음
  - 일반 네트워크와 분리된 전용망 사용
- 특징
  - 매우 빠른 속도
  - 높은 안정성
  - 구축 비용이 비쌈
  - 대규모 기업 데이터센터에서 주로 사용
- AWS 해당 서비스 : **EBS**
  - EC2에 붙이는 가상 블록 스토리지
  - EC2 종료해도 데이터 유지
  - 하나의 EC2에만 연결 가능
  - 용도 : OS 설치, DB 저장
4. **S3**
- (Simple Storage)
- 오브젝트 스토리지, 파일을 객체 단위로 저장
- 용량 제한 없음
- 인터넷으로 어디서든 접근 가능
- 파일시스템이 아닌 HTTP API로 접근
5. **스냅샷**
- 개념
  - 특정 시점의 스토리지 상태를 그대로 복사해두는 것
  - 장애 발생 시 해당 시점으로 복구 가능
  - 주로 EBS 스냅샷이 대표적이다.
- 증분 백업 방식
  - 처음에만 전체 저장, 이후엔 변경된 부분만 저장
  - 저장 공간 효율적
6. **RDS**
- (Relational Database Service)
- 개념
  - AWS에서 제공하는 관계형 데이터베이스 관리형 서비스
  - MySQL, PostgreSQL, MariaDB, Oracle 등 지원
- 특징
  - 자동 백업, 자동 패치, 장애 복구 AWS가 담당
  - **Multi-AZ** : 다른 AZ에 복제본을 두어 장애 시 자동 전환
  - **Read Replica** : 읽기 전용 복제본을 두어 DB 부하 분산
  - EC2에 직접 DB 설치하는 것과의 차이
    - EC2 직접 설치 : 백업/패치/복구 전부 직접 관리
    - RDS : AWS가 인프라 관리, 개발자는 쿼리에만 집중
</details>

<details>
<summary>AWS 네트워크</summary>

1. **VPC** (보충)
- 개념
  - AWS 클라우드 안에서 논리적으로 격리된 나만의 네트워크 공간
  - 실제 데이터센터의 네트워크를 가상화 한것
- 특징
  - 외부에서 허가 없이 접근 불가
  - 내부 IP 대역을 직접 설정 가능(CIDR)
  - **CIDR (Classless Inter-Domain Routing)**
    - IP 주소의 범위를 표기하는 방법
    - / 뒤 숫자로 네트워크 크기를 나타냄
    - 서브넷 마스크를 더 간결하게 표현한 현대 방식
    - 숫자가 클수록 범위 좁아지고, 작을수록 범위 넓어짐
2. **IGW**
- Internet Gateway
- 개념
  - VPC가 인터넷과 통신할 수 있게 해주는 출입문
  - VPC에 하나만 연결 가능
- 특징
  - Public Subnet의 리소스가 인터넷과 통신할 때 사용
  - 양방향 통신 가능
  - IGW 없으면 VPC는 인터넷X
3. **NAT Gateway**
- 개념
  - Private Subnet의 리소스가 인터넷으로 나가는 것만 허용하는 장치
  - Network Address Translation : 사설 IP를 공인 IP로 변환
- 특징
  - Outbound(나감) : 허용 -> 패키지 설치, 외부 API 호출 가능
  - Inbound(들어옴) : 차단 -> 외부에서 직접 접근 불가
  - Public Subnet 안에 위치
4. **라우팅 테이블**
- 개념
  - 네트워크 트래픽을 어디로 보낼지 규칙을 정의한 표
  - 서브넷마다 라우팅 테이블 연결
- 특징
    - 목적지 IP에 따라 트래픽 경로 결정
    - Public/Private Subnet마다 다른 규칙 적용
5. **Security Group**
- 개념
  - EC2 인스턴스 단위로 적용되는 가상 방화벽
  - 인바운드/아웃바운드 트래픽 규칙 설정
- 특징
  - Stateful 인바운드 허용하면 응답 트래픽은 자동 허용
  - 허용 규칙만 설정 가능(명시적 차단 규칙 없음)
  - 포트,프로토콜,IP 기준으로 제어
  - 여러 EC2에 동일한 보안 그룹 적용 가능
6. **NACL**
- Network Access Control List
- 개념
  - 서브넷 단위로 적용되는 추가 방화벽
  - Security Group 보다 상위 레벨
- 특징
  - Stateless -- 인바운드 허용해도 아웃바운드 따로 설정 필요
  - 허용/차단 규칙 둘 다 설정 가능
  - 규칙 번호 순서대로 적용
7. **ELB** (Elastic Load Balancer)
- 개념
  - 들어오는 네트워크 트래픽을 여러 서버에 자동으로 분산시켜주는 AWS 관리형 로드 밸런서
- 특징
  - **트래픽 분산** : 하나의 서버에 요청이 몰리지 않도록 여러 서버에 균등하게 나눠줌
  - **고가용성** : 특정 서버가 죽어도 다른 서버로 자동 우회
  - **헬스 체크** : 주기적으로 서버 상태를 확인하고, 비정상 서버는 자동으로 제외
  - **오토스케일링 연동** : 서버가 늘어나거나 줄어들면 ELB가 자동으로 대상 목록을 갱신
  - **SSL/TLS 종료** : HTTPS 암호화/복호화를 ELB가 대신 처리해서 서버 부담 감소
  - **관리형 서비스** : 직접 로드 밸런서를 구축할 필요 없이 AWS가 운영/유지보수 담당
- 종류
  - **ALB** (Application Load Balancer)
    - HTTP/HTTPS 트래픽 처리
    - URL 경로 기반 라우팅 가능
    - 웹 서버, API 서버에 적합
  - **NLB** (Network Load Balancer)
    - TCP/UDP 트래픽 처리
    - 매우 빠른 속도
    - 게임 서버, 실시간 서비스에 적합
8. **Route 53**
- 개념 
  - AWS의 DNS서비스
  - 도메인 이름을 IP 주소로 변환
- 특징
  - 도메인 구매/등록 가능
  - 헬스체크 기능 -- 서버 죽으면 다른 서버로 자동 전환
  - 지역 기반 라우팅 가능
9. **CloudFront**
- 개념
  - AWS의 CDN(Content Delivery Network) 서비스
  - 전 세계 Edge Location에 콘텐츠를 캐싱해서 빠르게 제공
  - 캐싱 : 자주 쓰는 데이터를 가까운 곳에 미리 저장해 두는것
- 특징
  - 정적파일 캐싱
  - 원본 서버 부하 감소
  - DDos 공격 방어 기능 포함
</details>
<details>
<summary>AWS Services</summary>

### Computing Service
1. **Auto Scaling** (자세히)
- 개념
  - 트래픽에 따라 EC2 인스턴스 수를 자동으로 조절하는 서비스
- 특징
  - 트래픽 증가 -> 자동 EC2 추가
  - 트래픽 감소 -> 자동 EC2 제거
  - 최소/최대 인스턴스 수 설정 가능
2. **ECS**와 **EKS**
- 개념
  - 컨테이너(Docker)를 실행/관리하는 서비스
- **특징**
- ECS
  - 기반 : AWS 자체
  - 설정 : 간단
  - 범용성 : AWS 종속
- EKS
  - 기반 : Kubernetes
  - 설정 : 복잡
  - 범용성 : 타 클라우드 이관 가능
3. **Elastic Beanstalk**
- 개념
  - 코드만 올리면 인프라 구성을 자동으로 해주는 **PaaS** 서비스
- 특징
  - EC2, ELB, Auto Scaling 자동 구성
  - 지원 언어
    - Python
    - Java
    - Node.js
    - Ruby
    - PHP
### Architecture

1. **3-Tier Architecture**
- 개념
  - 애플리케이션을 3개의 독립된 계층으로 분리해서 설계하는 구조
  - 각 계층은 자기 역할만 담당하고 서로 직접 통신하지 않음
- 구성
  - **Presentation Tier (프론트엔드)**
    - 사용자가 직접 보고 상호작용하는 화면
    - HTML, CSS, JavaScript로 구성
    - AWS : CloudFront, S3
  - **Application Tier (백엔드)**
    - 실제 기능 처리 담당 (로그인, 주문, 검색 등)
    - 프론트에서 요청 받아서 DB에 전달하거나 결과 반환
    - AWS : EC2, ELB, Auto Scaling, Elastic Beanstalk
  - **Data Tier (데이터베이스)**
    - 데이터를 저장하고 관리
    - 백엔드만 접근 가능, 외부에서 직접 접근 불가
    - AWS : RDS, S3
- 특징
  - 각 계층이 독립적이라 한 계층만 수정/확장 가능
  - DB는 Private Subnet에 위치해서 외부에서 직접 접근 불가 -> 보안 강화
  - Application Tier에 ELB + Auto Scaling 적용해서 트래픽 분산 가능
  - 실제 대부분의 웹 서비스에서 사용하는 표준 구조
- 흐름
  - 사용자 요청 -> Presentation -> Application -> Data -> 결과 반환

2. **2-Tier Architecture**
- 개념
  - 애플리케이션을 2개의 계층으로만 나눠서 설계하는 구조
  - 3-Tier에서 Application Tier가 없고 Client가 DB에 직접 접근
- 구성
  - **Client Tier**
    - 사용자 화면 + 비즈니스 로직을 같이 처리
    - 화면 표시와 기능 처리를 한 곳에서 담당
  - **Data Tier**
    - 데이터 저장 및 관리
    - AWS : RDS
- 특징
  - 구조가 단순해서 개발 속도 빠름
  - Client가 DB에 직접 접근하기 때문에 보안에 취약
  - 사용자가 많아지면 DB에 부하가 몰려서 성능 저하
  - 유지보수가 어려움 -> 기능 수정 시 클라이언트 전체 수정 필요
  - 소규모 내부 시스템에 적합

3. **2-Tier vs 3-Tier 비교**

| | 2-Tier | 3-Tier |
|---|---|---|
| 구조 | Client - DB | Client - Server - DB |
| 보안 | 취약 (DB 직접 접근) | 강함 (DB 간접 접근) |
| 확장성 | 낮음 | 높음 |
| 개발 속도 | 빠름 | 상대적으로 느림 |
| 적합한 규모 | 소규모 | 중대규모 |

### Computing Service & Networking Service
1. Computing Service
- EC2
- Lambda
- Auto Scaling
- ECS
- EKS
- Elastic Beanstalk
2. Networking Service
- VPC
- IGW
- NAT Gateway
- 라우팅 테이블
- Security Group
- NACL
- ELB
- Route 53
- CloudFront
</details>