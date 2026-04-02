# AWS 기초 개념 정리

> 작성일: 2026-04-01  
> 대상: 클라우드/DevOps 입문~중급 학습자  
> 범위: AWS 전체 개요 — 글로벌 인프라부터 주요 서비스 카테고리까지

---

## 목차

1. [AWS란?](#1-aws란)
2. [글로벌 인프라](#2-글로벌-인프라)
3. [컴퓨팅 (Compute)](#3-컴퓨팅-compute)
4. [스토리지 (Storage)](#4-스토리지-storage)
5. [데이터베이스 (Database)](#5-데이터베이스-database)
6. [네트워킹 및 콘텐츠 전달 (Networking & CDN)](#6-네트워킹-및-콘텐츠-전달-networking--cdn)
7. [보안, 자격 증명 및 규정 준수 (Security, Identity & Compliance)](#7-보안-자격-증명-및-규정-준수-security-identity--compliance)
8. [관리 및 거버넌스 (Management & Governance)](#8-관리-및-거버넌스-management--governance)
9. [컨테이너 (Containers)](#9-컨테이너-containers)
10. [서버리스 (Serverless)](#10-서버리스-serverless)
11. [개발자 도구 및 CI/CD (Developer Tools)](#11-개발자-도구-및-cicd-developer-tools)
12. [AI/ML 및 분석 (AI/ML & Analytics)](#12-aiml-및-분석-aiml--analytics)
13. [마이그레이션 및 현대화 (Migration & Modernization)](#13-마이그레이션-및-현대화-migration--modernization)
14. [요금 체계 및 프리 티어](#14-요금-체계-및-프리-티어)
15. [아키텍처 패턴 예시](#15-아키텍처-패턴-예시)
16. [참고 자료](#16-참고-자료)

---

## 1. AWS란?

**Amazon Web Services(AWS)** 는 Amazon이 제공하는 클라우드 컴퓨팅 플랫폼이다.  
2006년 S3와 EC2 출시를 시작으로, 현재 **200개 이상의 완전관리형 서비스**를 제공하며, 전 세계 클라우드 시장 점유율 1위를 유지하고 있다.

### 클라우드 컴퓨팅의 핵심 모델

| 모델 | 설명 | AWS 예시 |
|------|------|----------|
| **IaaS** (Infrastructure as a Service) | 가상 서버, 스토리지, 네트워크 등 인프라 제공 | EC2, VPC, EBS |
| **PaaS** (Platform as a Service) | 애플리케이션 개발 플랫폼 제공 | Elastic Beanstalk, App Runner |
| **SaaS** (Software as a Service) | 완성된 소프트웨어를 서비스로 제공 | Amazon WorkSpaces, Amazon Chime |
| **FaaS** (Function as a Service) | 서버리스 함수 실행 환경 제공 | AWS Lambda |

### 클라우드의 6가지 이점 (AWS Well-Architected 기준)

1. **자본 지출 → 가변 지출 전환**: 선투자 없이 사용한 만큼만 지불
2. **규모의 경제**: AWS의 대규모 인프라를 통한 저비용 혜택
3. **용량 추정 불필요**: 필요에 따라 즉시 스케일 업/다운
4. **속도와 민첩성 향상**: 몇 분 만에 글로벌 인프라 구축
5. **데이터센터 운영 비용 제거**: 인프라 관리를 AWS에 위임
6. **글로벌 배포**: 전 세계 리전에 빠르게 서비스 배포

---

## 2. 글로벌 인프라

AWS의 글로벌 인프라는 **리전(Region)**, **가용 영역(AZ)**, **엣지 로케이션(Edge Location)** 세 계층으로 구성된다.

### 리전 (Region)

- 전 세계 **39개 리전** 운영 중 (2026년 기준), 2개 추가 예정
- 각 리전은 지리적으로 독립된 영역으로, **최소 3개의 AZ**를 포함
- 리전 선택 기준:
  - **지연 시간(Latency)**: 사용자와 가까운 리전 선택
  - **규정 준수(Compliance)**: 데이터 주권 법규 (예: 한국 개인정보보호법)
  - **서비스 가용성**: 모든 서비스가 모든 리전에 있지는 않음
  - **비용**: 리전마다 가격이 다름 (US East가 보통 가장 저렴)

> **한국 리전**: `ap-northeast-2` (서울), 4개 AZ 운영

### 가용 영역 (Availability Zone, AZ)

- 전 세계 **123개 AZ** 운영 중
- 각 AZ는 하나 이상의 **독립된 데이터센터**로 구성
- 독립된 전원, 냉각, 물리 보안 시스템 보유
- AZ 간 **저지연 고대역폭 네트워크**로 연결
- **고가용성(HA) 설계**: 애플리케이션을 2개 이상의 AZ에 분산 배치 권장

| AZ 식별자 | 내부 코드 | 구성 |
|-----------|-----------|------|
| AZ-a | apne2-az1 | 데이터센터 클러스터 |
| AZ-b | apne2-az2 | 데이터센터 클러스터 |
| AZ-c | apne2-az3 | 데이터센터 클러스터 |
| AZ-d | apne2-az4 | 데이터센터 클러스터 |

### 엣지 로케이션 (Edge Location)

- **400개 이상**의 전 세계 분산 캐시 서버
- **CloudFront**(CDN), **Route 53**(DNS), **AWS WAF** 등에서 사용
- 사용자에게 가장 가까운 지점에서 콘텐츠를 제공하여 지연 시간 최소화

---

## 3. 컴퓨팅 (Compute)

### Amazon EC2 (Elastic Compute Cloud)

AWS의 핵심 컴퓨팅 서비스. 가상 서버(인스턴스)를 제공한다.

#### 인스턴스 유형

| 패밀리 | 용도 | 예시 |
|--------|------|------|
| **T** (범용, 버스트) | 웹 서버, 개발 환경 | t3.micro, t4g.medium |
| **M** (범용) | 균형 잡힌 워크로드 | m7i.large, m7g.xlarge |
| **C** (컴퓨팅 최적화) | 배치 처리, HPC | c7g.2xlarge |
| **R** (메모리 최적화) | 인메모리 DB, 캐싱 | r7g.large |
| **G/P** (GPU) | ML 훈련, 그래픽 렌더링 | g5.xlarge, p5.48xlarge |
| **I** (스토리지 최적화) | 높은 IOPS 워크로드 | i4i.large |

> **Graviton**: AWS 자체 설계 ARM 기반 프로세서. `g` 접미사 인스턴스(t4g, m7g 등)에 적용되며, x86 대비 가성비가 우수하다. 최신 세대는 **Graviton5** (2025 re:Invent 프리뷰).

#### 구매 옵션

| 옵션 | 특징 | 할인율 | 적합한 경우 |
|------|------|--------|------------|
| **온디맨드** | 초 단위 과금, 약정 없음 | - | 예측 불가능한 워크로드 |
| **예약 인스턴스 (RI)** | 1~3년 약정 | 최대 72% | 안정적 워크로드 |
| **Savings Plans** | 시간당 사용량 약정 | 최대 72% | 유연한 약정 선호 시 |
| **스팟 인스턴스** | 미사용 용량 활용 | 최대 90% | 중단 허용 가능한 작업 |
| **전용 호스트** | 물리 서버 전용 사용 | 다양 | 라이선스/규정 준수 |

#### 주요 관련 개념

- **AMI (Amazon Machine Image)**: EC2 인스턴스의 템플릿. OS, 소프트웨어, 설정을 포함
- **보안 그룹 (Security Group)**: 인스턴스 레벨 방화벽 (Stateful)
- **키 페어 (Key Pair)**: SSH 접속을 위한 공개키/개인키 쌍
- **EBS (Elastic Block Store)**: EC2에 연결하는 블록 스토리지 볼륨
- **Auto Scaling**: 트래픽에 따라 인스턴스 수를 자동 조절

### AWS Lambda

- **서버리스 컴퓨팅** 서비스: 서버 관리 없이 코드 실행
- 이벤트 기반 실행 (S3 업로드, API 호출, 스케줄 등)
- 지원 언어: Python, Node.js, Java, Go, .NET, Ruby 등
- 최대 실행 시간: 15분
- 동시 실행 수: 최대 10,000+ (리전별 상이)
- 과금: 요청 수 + 실행 시간(GB-초) 기준
- **2025 신규**: Lambda Durable Functions (다단계 워크플로우, 최대 1년 대기), Lambda Managed Instances (EC2 컴퓨팅에서 Lambda 함수 실행)

### 기타 컴퓨팅 서비스

| 서비스 | 설명 |
|--------|------|
| **Elastic Beanstalk** | PaaS — 코드만 업로드하면 인프라 자동 구성 (EC2 + ALB + ASG) |
| **AWS App Runner** | 컨테이너/소스코드에서 바로 웹 앱 배포. Beanstalk보다 더 간편 |
| **Amazon Lightsail** | 간단한 VPS 서비스. 고정 요금제 (소규모 웹사이트, 블로그 등) |
| **AWS Batch** | 대규모 배치 컴퓨팅 작업 스케줄링 및 실행 |
| **AWS Outposts** | AWS 인프라를 온프레미스에 설치하여 하이브리드 구성 |

---

## 4. 스토리지 (Storage)

### Amazon S3 (Simple Storage Service)

AWS의 핵심 객체 스토리지. **내구성 99.999999999% (11 nines)**.

#### 스토리지 클래스

| 클래스 | 용도 | 특징 |
|--------|------|------|
| **S3 Standard** | 자주 접근하는 데이터 | 높은 가용성, 낮은 지연 |
| **S3 Intelligent-Tiering** | 접근 패턴 변동 | 자동 계층 이동, 비용 최적화 |
| **S3 Standard-IA** | 비정기 접근 | 낮은 저장 비용, 조회 비용 발생 |
| **S3 One Zone-IA** | 비정기 접근 (단일 AZ) | 더 저렴, AZ 장애 시 데이터 손실 위험 |
| **S3 Glacier Instant Retrieval** | 아카이브 (즉시 접근) | 분기 1회 접근 수준 |
| **S3 Glacier Flexible Retrieval** | 아카이브 | 수 분~수 시간 복원 |
| **S3 Glacier Deep Archive** | 장기 아카이브 | 가장 저렴, 12~48시간 복원 |

#### 주요 기능

- **버전 관리 (Versioning)**: 객체의 모든 버전을 보관
- **수명 주기 정책 (Lifecycle Policy)**: 자동으로 스토리지 클래스 전환 또는 삭제
- **교차 리전 복제 (CRR)**: 다른 리전에 자동 복제 (재해 복구)
- **정적 웹 호스팅**: S3 버킷을 정적 웹사이트로 호스팅
- **S3 Vectors (2025 신규)**: 벡터 데이터 저장 및 쿼리, 인덱스당 최대 20억 벡터 지원
- **S3 Tables (2025 신규)**: Apache Iceberg 포맷의 테이블 데이터 최적화 스토리지

### 기타 스토리지 서비스

| 서비스 | 유형 | 설명 |
|--------|------|------|
| **EBS** (Elastic Block Store) | 블록 스토리지 | EC2 인스턴스에 연결, SSD/HDD 선택 |
| **EFS** (Elastic File System) | 파일 스토리지 | 여러 EC2 인스턴스에서 공유, NFS 프로토콜 |
| **FSx** | 파일 스토리지 | Windows File Server, Lustre, NetApp ONTAP 등 |
| **AWS Storage Gateway** | 하이브리드 | 온프레미스와 AWS 스토리지 연결 |
| **AWS Backup** | 백업 | 중앙화된 백업 관리 |

---

## 5. 데이터베이스 (Database)

### 관계형 데이터베이스

| 서비스 | 설명 |
|--------|------|
| **Amazon RDS** | 관리형 관계형 DB. MySQL, PostgreSQL, MariaDB, Oracle, SQL Server 지원. 자동 백업, Multi-AZ 고가용성, Read Replica 제공 |
| **Amazon Aurora** | AWS 자체 설계 DB 엔진. MySQL/PostgreSQL 호환. 표준 대비 최대 5배(MySQL)/3배(PostgreSQL) 성능. 스토리지 자동 확장 (최대 128TB) |

### NoSQL 데이터베이스

| 서비스 | 설명 |
|--------|------|
| **Amazon DynamoDB** | 완전관리형 키-값/문서 DB. 한 자릿수 밀리초 지연. 자동 확장. DAX(캐시) 지원 |
| **Amazon DocumentDB** | MongoDB 호환 문서 DB |
| **Amazon Keyspaces** | Apache Cassandra 호환 |

### 기타 데이터베이스 서비스

| 서비스 | 용도 |
|--------|------|
| **Amazon ElastiCache** | Redis/Memcached 기반 인메모리 캐시 |
| **Amazon Neptune** | 그래프 데이터베이스 |
| **Amazon Timestream** | 시계열 데이터베이스 |
| **Amazon QLDB** | 원장(Ledger) 데이터베이스 — 변경 불가능한 기록 |
| **Amazon Redshift** | 데이터 웨어하우스 — 대규모 분석 쿼리 |

> **2025 신규**: AWS Database Saving Plan 도입 — 1년 약정으로 DB 서비스 최대 35% 할인 (RDS, Aurora, DynamoDB, ElastiCache 등 포함)

---

## 6. 네트워킹 및 콘텐츠 전달 (Networking & CDN)

### Amazon VPC (Virtual Private Cloud)

AWS 네트워킹의 핵심. **논리적으로 격리된 가상 네트워크**를 정의하고 제어한다.

#### VPC 핵심 구성 요소

| 서브넷 / 구성 요소 | CIDR | 가용 영역 | 포함 리소스 |
|--------------------|------|-----------|-------------|
| 퍼블릭 서브넷 | 10.0.1.0/24 | AZ-a | EC2 웹 서버 (퍼블릭 IP 보유), NAT Gateway |
| 프라이빗 서브넷 | 10.0.2.0/24 | AZ-a | EC2 앱 서버 (프라이빗 IP만) |
| 퍼블릭 서브넷 | 10.0.3.0/24 | AZ-b | EC2 웹 서버 |
| 프라이빗 서브넷 | 10.0.4.0/24 | AZ-b | RDS 데이터베이스 |
| Internet Gateway (IGW) | — | VPC 전체 | 인터넷 연결 게이트웨이 |
| Route Table | — | VPC 전체 | 트래픽 라우팅 규칙 |
| Network ACL | — | 서브넷 레벨 | 방화벽 (Stateless) |

| 구성 요소 | 설명 |
|-----------|------|
| **서브넷 (Subnet)** | VPC 내 IP 주소 범위. 하나의 AZ에 속함. 퍼블릭/프라이빗 구분 |
| **라우팅 테이블 (Route Table)** | 서브넷의 트래픽 경로 결정 |
| **인터넷 게이트웨이 (IGW)** | VPC와 인터넷 간 통신 게이트웨이 |
| **NAT 게이트웨이** | 프라이빗 서브넷 → 인터넷 단방향 연결 (인바운드 차단) |
| **보안 그룹** | 인스턴스 레벨 방화벽. **Stateful** (응답 트래픽 자동 허용) |
| **네트워크 ACL** | 서브넷 레벨 방화벽. **Stateless** (인바운드/아웃바운드 별도 설정) |
| **VPC 피어링** | 두 VPC 간 프라이빗 통신. CIDR 겹침 불가 |
| **VPC 엔드포인트** | AWS 서비스에 프라이빗 연결 (인터넷 경유 없이) |
| **Transit Gateway** | 여러 VPC와 온프레미스 네트워크를 중앙 허브로 연결 |

#### 보안 그룹 vs 네트워크 ACL

| 항목 | 보안 그룹 | 네트워크 ACL |
|------|-----------|-------------|
| 적용 범위 | 인스턴스(ENI) 레벨 | 서브넷 레벨 |
| 상태 | Stateful | Stateless |
| 규칙 | 허용 규칙만 | 허용 + 거부 규칙 |
| 평가 방식 | 모든 규칙 평가 | 번호 순서대로 평가 |
| 기본 동작 | 모든 인바운드 차단, 모든 아웃바운드 허용 | 모든 트래픽 허용 (기본 ACL) |

### 기타 네트워킹 서비스

| 서비스 | 설명 |
|--------|------|
| **Amazon Route 53** | 관리형 DNS 서비스. 도메인 등록, 라우팅 정책(지연, 가중치, 장애 조치 등) |
| **Amazon CloudFront** | CDN — 400+ 엣지 로케이션에서 콘텐츠 캐싱. DDoS 방어 통합 |
| **Elastic Load Balancing (ELB)** | 트래픽 분산. ALB(HTTP/HTTPS), NLB(TCP/UDP), GLB(서드파티 어플라이언스) |
| **AWS Direct Connect** | 온프레미스와 AWS 간 전용 네트워크 연결 (인터넷 미경유) |
| **AWS VPN** | Site-to-Site VPN 또는 Client VPN. 암호화된 터널 연결 |
| **AWS Global Accelerator** | AWS 글로벌 네트워크를 통한 성능 최적화 |

---

## 7. 보안, 자격 증명 및 규정 준수 (Security, Identity & Compliance)

### AWS 공동 책임 모델 (Shared Responsibility Model)

| 구분 | 책임 범위 |
|------|----------|
| **고객 책임** ("IN the Cloud") | 데이터 암호화, IAM 설정, OS 패치, 보안 그룹 설정, 애플리케이션 보안, 네트워크 설정 |
| **AWS 책임** ("OF the Cloud") | 물리 인프라, 하드웨어, 네트워크 인프라, 호스트 OS, 가상화 계층, 데이터센터 보안 |

### AWS IAM (Identity and Access Management)

AWS 리소스에 대한 **접근 제어**를 관리하는 핵심 서비스.

| 개념 | 설명 |
|------|------|
| **루트 사용자** | 계정 생성 시의 최초 사용자. 모든 권한 보유. 일상 작업에 사용 금지 |
| **IAM 사용자** | 개인/서비스별 자격 증명. 콘솔/API 접근 |
| **IAM 그룹** | 사용자 묶음. 그룹에 정책 연결 → 소속 사용자에 일괄 적용 |
| **IAM 역할 (Role)** | 임시 자격 증명. EC2, Lambda 등 서비스에 권한 부여 시 사용 |
| **IAM 정책 (Policy)** | JSON 형식의 권한 문서. Effect(Allow/Deny), Action, Resource 정의 |
| **MFA** | 다중 인증. 루트 사용자와 관리자에 필수 |

#### IAM 정책 예시

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

> **최소 권한 원칙 (Principle of Least Privilege)**: 작업에 꼭 필요한 권한만 부여

### 기타 보안 서비스

| 서비스 | 설명 |
|--------|------|
| **AWS KMS** | 암호화 키 관리 (Key Management Service) |
| **AWS Secrets Manager** | DB 자격증명, API 키 등 비밀 정보 관리 및 자동 교체 |
| **AWS Certificate Manager** | SSL/TLS 인증서 무료 발급 및 관리 |
| **Amazon GuardDuty** | 위협 탐지 — 계정/워크로드의 악의적 활동 모니터링 |
| **Amazon Inspector** | 취약점 스캔 — EC2, 컨테이너 이미지 자동 검사 |
| **AWS Security Hub** | 보안 상태 통합 대시보드 |
| **AWS WAF** | 웹 애플리케이션 방화벽 — SQL 인젝션, XSS 등 방어 |
| **AWS Shield** | DDoS 방어 (Standard: 무료, Advanced: 유료) |
| **AWS CloudTrail** | API 호출 기록 — 감사 및 컴플라이언스 용도 |
| **AWS Config** | 리소스 구성 변경 추적 및 규정 준수 자동 평가 |
| **IAM Identity Center** (구 SSO) | 다중 계정 SSO 관리 |

---

## 8. 관리 및 거버넌스 (Management & Governance)

| 서비스 | 설명 |
|--------|------|
| **Amazon CloudWatch** | 모니터링 및 관측성 — 메트릭, 로그, 알람, 대시보드. Anomaly Detection으로 AI 기반 이상 탐지 지원 |
| **AWS CloudFormation** | **Infrastructure as Code (IaC)** — YAML/JSON 템플릿으로 인프라 프로비저닝 |
| **AWS CDK** | 프로그래밍 언어(TypeScript, Python 등)로 IaC 작성. CloudFormation 생성 |
| **AWS Systems Manager** | EC2 인스턴스 관리, 패치, 파라미터 스토어, Run Command 등 |
| **AWS Organizations** | 다중 계정 관리 및 통합 결제. SCP(Service Control Policy)로 권한 제한 |
| **AWS Trusted Advisor** | 비용, 성능, 보안, 장애 내성, 서비스 한도 모범 사례 검사 |
| **AWS Control Tower** | 다중 계정 환경의 거버넌스 자동화 (Landing Zone) |

### IaC (Infrastructure as Code) 개념

코드로 인프라를 정의하여 **버전 관리, 재현 가능성, 자동화**를 실현한다.

```yaml
# CloudFormation 예시 — EC2 인스턴스 생성
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t3.micro
      ImageId: ami-0abcdef1234567890
      Tags:
        - Key: Name
          Value: MyWebServer
```

> **Terraform**은 AWS 공식 도구는 아니지만, 멀티클라우드/하이브리드 환경에서 널리 사용되는 IaC 도구다. AWS CloudFormation과 함께 DevOps 현장에서 양대 산맥을 이룬다.

---

## 9. 컨테이너 (Containers)

### 서비스 비교

| 서비스 | 설명 | 적합한 경우 |
|--------|------|------------|
| **Amazon ECS** | AWS 네이티브 컨테이너 오케스트레이션 | AWS 생태계에 집중, Kubernetes 불필요 시 |
| **Amazon EKS** | 관리형 Kubernetes 서비스 | K8s 전문성 보유, 멀티클라우드/하이브리드 시 |
| **AWS Fargate** | 서버리스 컨테이너 실행 엔진 (ECS/EKS 모두 지원) | 인프라 관리 없이 컨테이너 실행 |
| **Amazon ECR** | 컨테이너 이미지 레지스트리 | Docker 이미지 저장 및 관리 |

### ECS vs EKS 심화 비교

| 항목 | ECS | EKS |
|------|-----|-----|
| 오케스트레이터 | AWS 자체 | Kubernetes |
| 최소 배포 단위 | Task (태스크) | Pod (파드) |
| 학습 곡선 | 낮음 | 높음 |
| 서드파티 통합 | 제한적 | 풍부 (K8s 생태계 전체) |
| 컨트롤 플레인 관리 | AWS 완전 관리 | AWS가 관리 (워커 노드는 선택) |
| GitOps 지원 | CodeDeploy 통합 | ArgoCD, Flux 등 |
| 비용 | 컨트롤 플레인 무료 | 클러스터당 $0.10/시간 |

> **Fargate 활용 패턴**: ECS + Fargate 조합이 가장 간편. EC2 인스턴스 관리 없이 컨테이너 실행. 비용은 vCPU + 메모리 사용량 기준 초 단위 과금.

### 2025 신규: EKS Capabilities

EKS Capabilities가 정식 출시되어, 워크로드 오케스트레이션과 클라우드 리소스 관리를 완전 관리형 플랫폼 기능으로 제공한다. 인프라 유지보수 부담이 크게 줄었다.

---

## 10. 서버리스 (Serverless)

서버를 직접 관리하지 않고 코드와 비즈니스 로직에 집중하는 아키텍처.

### 주요 서버리스 서비스

| 서비스 | 역할 |
|--------|------|
| **AWS Lambda** | 이벤트 기반 함수 실행 |
| **Amazon API Gateway** | RESTful/WebSocket API 생성 및 관리 |
| **Amazon DynamoDB** | 서버리스 NoSQL 데이터베이스 |
| **Amazon S3** | 서버리스 객체 스토리지 |
| **AWS Fargate** | 서버리스 컨테이너 실행 |
| **Amazon SQS** | 메시지 큐 — 서비스 간 비동기 통신 |
| **Amazon SNS** | 알림 서비스 — Pub/Sub 메시징 |
| **Amazon EventBridge** | 이벤트 버스 — 이벤트 기반 아키텍처의 중심 |
| **AWS Step Functions** | 워크플로우 오케스트레이션 — Lambda 함수 체이닝 |

### 서버리스 아키텍처 패턴 예시

사용자 → API Gateway → Lambda → DynamoDB

Lambda 실행 중 추가 연동:
- Lambda → S3 (파일 저장)
- Lambda → SNS (알림 발송)

---

## 11. 개발자 도구 및 CI/CD (Developer Tools)

### AWS 네이티브 CI/CD 파이프라인

CodeCommit (소스) → CodeBuild (빌드/테스트) → CodeDeploy (배포)

전체 파이프라인은 **CodePipeline**이 오케스트레이션하며, CodeCommit → CodeBuild → CodeDeploy의 각 단계를 통합 관리한다.

| 서비스 | 역할 | 비고 |
|--------|------|------|
| **AWS CodeCommit** | Git 리포지토리 (프라이빗) | GitHub/GitLab 대안. 2024년 이후 신규 사용자 제한됨 |
| **AWS CodeBuild** | 빌드 및 테스트 자동화 | Docker 이미지 빌드, 단위 테스트 실행 |
| **AWS CodeDeploy** | 애플리케이션 배포 자동화 | EC2, Lambda, ECS 지원. Blue/Green, Rolling 배포 |
| **AWS CodePipeline** | CI/CD 파이프라인 오케스트레이션 | GitHub, CodeBuild, CodeDeploy 등 연결 |
| **AWS CodeArtifact** | 패키지 관리 (npm, pip, Maven) | 프라이빗 패키지 레포지토리 |

### 배포 전략

| 전략 | 설명 | 위험도 |
|------|------|--------|
| **In-place (Rolling)** | 기존 인스턴스에 순차 배포 | 중간 |
| **Blue/Green** | 새 환경에 배포 후 트래픽 전환 | 낮음 (빠른 롤백 가능) |
| **Canary** | 소량 트래픽으로 먼저 테스트 후 점진 확대 | 매우 낮음 |
| **Immutable** | 새 인스턴스 그룹 생성 후 전환 | 낮음 |

### 2025 신규: AWS DevOps Agent (프리뷰)

자율적인 온콜 엔지니어 역할. CloudWatch, GitHub, ServiceNow 등의 데이터를 분석하여 근본 원인을 파악하고 사고 대응을 자동 조정한다.

### 실무 CI/CD 구성 (2026 현황)

2026년 현재 실무에서는 AWS 네이티브 도구 외에도 아래 조합이 널리 사용된다:
- **GitHub Actions + AWS**: GitHub 리포지토리와 OIDC 인증으로 AWS 연동
- **GitLab CI + AWS**: OIDC 인증 기반 배포
- **ArgoCD / Flux + EKS**: GitOps 패턴의 Kubernetes 배포
- **Terraform + CI/CD**: 인프라 변경의 PR 기반 리뷰 워크플로우

---

## 12. AI/ML 및 분석 (AI/ML & Analytics)

### AI/ML 서비스

| 서비스 | 설명 |
|--------|------|
| **Amazon SageMaker AI** | ML 모델 개발 통합 플랫폼 — 빌드, 훈련, 배포. 서버리스 미세 조정 지원 (2025 신규) |
| **Amazon Bedrock** | 생성형 AI 파운데이션 모델 서비스. Claude, Llama, Mistral 등 다양한 모델 제공 |
| **Amazon Bedrock AgentCore** | AI 에이전트 배치 플랫폼 — 정책 통제, 품질 모니터링 기능 (2025 신규) |
| **Amazon Rekognition** | 이미지/비디오 분석 (얼굴 인식, 객체 탐지) |
| **Amazon Transcribe** | 음성 → 텍스트 변환 |
| **Amazon Polly** | 텍스트 → 음성 변환 |
| **Amazon Comprehend** | 자연어 처리 (NLP) — 감정 분석, 키워드 추출 |
| **Amazon Translate** | 기계 번역 |
| **AWS Trainium / Inferentia** | ML 전용 칩. 훈련(Trainium)/추론(Inferentia) 최적화. Trainium3(3nm) 발표 (2025) |

### 분석 서비스

| 서비스 | 설명 |
|--------|------|
| **Amazon Redshift** | 페타바이트급 데이터 웨어하우스 |
| **Amazon Athena** | S3 데이터를 SQL로 직접 쿼리 (서버리스) |
| **Amazon Kinesis** | 실시간 스트리밍 데이터 수집 및 분석 |
| **AWS Glue** | ETL(추출-변환-적재) 서비스. 데이터 카탈로그 |
| **Amazon QuickSight** | BI 대시보드 및 시각화 |
| **Amazon OpenSearch Service** | 검색 및 로그 분석. GPU 가속 벡터 DB 지원 (2025 신규) |
| **Amazon EMR** | 빅데이터 프레임워크 (Spark, Hadoop) 관리형 클러스터 |

---

## 13. 마이그레이션 및 현대화 (Migration & Modernization)

| 서비스 | 설명 |
|--------|------|
| **AWS Migration Hub** | 마이그레이션 진행 상황 중앙 추적 |
| **AWS Application Migration Service** | 서버를 AWS로 리프트 앤 시프트(Lift & Shift) |
| **AWS Database Migration Service (DMS)** | 데이터베이스 마이그레이션. 동종/이종 DB 간 전환 |
| **AWS Schema Conversion Tool** | DB 스키마 자동 변환 (Oracle → Aurora 등) |
| **AWS Snow Family** | 물리적 디바이스를 통한 대규모 데이터 이전. Snowball Edge, Snowcone 등 |
| **AWS Transfer Family** | SFTP, FTPS, FTP를 통한 S3/EFS 파일 전송 |
| **AWS Transform (2025 신규)** | AI 기반 코드 현대화 — 레거시 코드를 클라우드 네이티브로 자동 전환 |

### 마이그레이션 전략 (6R)

| 전략 | 설명 |
|------|------|
| **Rehost** (리호스트) | 그대로 클라우드로 이전 (Lift & Shift) |
| **Replatform** (리플랫폼) | 일부 최적화 후 이전 (Lift, Tinker & Shift) |
| **Refactor/Re-architect** | 클라우드 네이티브로 재설계 |
| **Repurchase** | SaaS로 전환 (예: CRM → Salesforce) |
| **Retire** | 사용 중단 |
| **Retain** | 현재 상태 유지 (추후 이전) |

---

## 14. 요금 체계 및 프리 티어

### 과금 원칙

- **사용한 만큼 지불 (Pay-as-you-go)**
- **예약 시 할인**: 장기 약정으로 비용 절감
- **많이 쓸수록 할인**: 사용량 증가 시 단위 비용 감소

### AWS 프리 티어 (3가지 유형)

| 유형 | 설명 | 예시 |
|------|------|------|
| **12개월 무료** | 계정 생성 후 12개월간 | EC2 t2.micro 750시간/월, S3 5GB, RDS 750시간/월 |
| **항상 무료** | 기간 제한 없음 | Lambda 100만 요청/월, DynamoDB 25GB, SNS 100만 게시 |
| **단기 체험** | 특정 기간 체험 | SageMaker 등 일부 서비스 |

### 비용 관리 도구

| 도구 | 설명 |
|------|------|
| **AWS Cost Explorer** | 비용 시각화 및 예측 |
| **AWS Budgets** | 예산 설정 및 알림 |
| **AWS Pricing Calculator** | 사전 비용 산정 |
| **Cost Allocation Tags** | 태그 기반 비용 분류 |

---

## 15. 아키텍처 패턴 예시

### 패턴 1: 전통적 3-Tier 웹 애플리케이션

사용자 → Route 53 (DNS) → CloudFront (CDN) → ALB (로드밸런서) → EC2 Auto Scaling Group (웹/앱 서버) → RDS Multi-AZ (데이터베이스) → ElastiCache (캐시)

### 패턴 2: 서버리스 웹 애플리케이션

- 프론트엔드: 사용자 → CloudFront → S3 (정적 프론트엔드)
- 백엔드: 사용자 → API Gateway → Lambda (백엔드 로직)
  - Lambda → DynamoDB (데이터)
  - Lambda → SQS → Lambda (비동기 처리)
  - Lambda → SNS (알림)

### 패턴 3: 컨테이너 기반 마이크로서비스

사용자 → ALB → ECS/Fargate (서비스 A, B, C)

- 서비스 간 통신: Service Discovery / App Mesh
- 데이터 계층: RDS, DynamoDB, S3
- 관측성: CloudWatch + X-Ray

### 패턴 4: DevOps CI/CD 파이프라인

개발자 → GitHub (소스) → CodePipeline (오케스트레이션) → CodeBuild (빌드 + 테스트 + 보안 스캔) → ECR (이미지 저장) → CodeDeploy (Blue/Green 배포) → CloudWatch (모니터링 + 알람) → SNS (배포 알림)

---

## 16. 참고 자료

| 리소스 | URL |
|--------|-----|
| AWS 공식 문서 | https://docs.aws.amazon.com/ |
| AWS 서비스 카테고리 | https://docs.aws.amazon.com/whitepapers/latest/aws-overview/amazon-web-services-cloud-platform.html |
| AWS 글로벌 인프라 | https://aws.amazon.com/about-aws/global-infrastructure/ |
| AWS 네트워킹 기초 | https://aws.amazon.com/getting-started/aws-networking-essentials/ |
| AWS 프리 티어 | https://aws.amazon.com/free/ |
| AWS Well-Architected Framework | https://aws.amazon.com/architecture/well-architected/ |
| AWS re:Invent 2025 주요 발표 | https://aws.amazon.com/ko/blogs/korea/top-announcements-of-aws-reinvent-2025/ |
| AWS DevOps Best Practices 2026 | https://kodekloud.com/blog/aws-devops-best-practices-in-2026/ |
| AWS 서비스별 개발자 가이드 2026 | https://axalingroup.com/blogs/20-aws-services-every-developer-should-know-2026-dekx6586x |

---

> 이 문서는 2026년 4월 기준으로 작성되었습니다. AWS는 지속적으로 서비스를 추가/변경하므로, 공식 문서를 함께 참고하세요.
