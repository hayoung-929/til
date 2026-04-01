# DevOps 개념 정리
---

## 목차

1. [DevOps 개요](#1-devops-개요)
2. [DevOps 라이프사이클 (무한루프)](#2-devops-라이프사이클-무한루프)
3. [DevOps 핵심 실천 방법 (Practices)](#3-devops-핵심-실천-방법-practices)
4. [DevOps 도구 생태계](#4-devops-도구-생태계)
5. [DevOps 문화와 조직](#5-devops-문화와-조직)
6. [DevOps 핵심 개념 심화](#6-devops-핵심-개념-심화)
7. [DevOps 메트릭과 KPI](#7-devops-메트릭과-kpi)
8. [DevOps 실무 시나리오](#8-devops-실무-시나리오)
9. [DevOps 엔지니어 로드맵](#9-devops-엔지니어-로드맵)
10. [참고 자료 및 링크](#10-참고-자료-및-링크)

---

## 1. DevOps 개요

### 1.1 DevOps 정의 및 역사

**DevOps**는 **Dev**elopment(개발)와 **Op**eration**s**(운영)의 합성어로, 소프트웨어 개발팀과 IT 운영팀 간의 협업을 강화하고 소프트웨어 배포 주기를 단축하는 문화적·기술적 접근 방식이다.

단순히 도구 세트나 방법론이 아니라 **철학이자 문화**다. 사람, 프로세스, 기술을 결합하여 더 빠르고 안정적으로 가치를 고객에게 전달하는 것을 목표로 한다.

#### DevOps 역사 타임라인

```
2007  Patrick Debois, 벨기에에서 Agile Infrastructure 개념 제창
        ↓
2008  Agile Conference에서 "Agile Infrastructure & Operations" 발표
        ↓
2009  첫 번째 DevOpsDays 컨퍼런스 개최 (벨기에 겐트)
      Flickr "10 deploys per day" 발표 — John Allspaw & Paul Hammond
        ↓
2010  "The Phoenix Project" 개념의 전신 등장
        ↓
2013  "The Phoenix Project" (Gene Kim 외) 출간
      Docker 0.1 릴리스 — 컨테이너 혁명 시작
        ↓
2014  "The DevOps Handbook" 작업 시작
      Kubernetes 구글에서 오픈소스 공개
        ↓
2016  "The DevOps Handbook" (Gene Kim, Jez Humble 외) 출간
      DORA(DevOps Research and Assessment) 연구 시작
        ↓
2018  Google, DORA 인수 — DevOps 메트릭 연구 공식화
        ↓
2020  Platform Engineering 개념 부상
        ↓
2023  AI-assisted DevOps (GitHub Copilot, AI Ops) 확산
```

### 1.2 DevOps가 등장한 배경

#### 전통적 개발 방식의 문제점

전통적인 소프트웨어 개발에서는 **사일로(Silo)** 현상이 만연했다. 개발팀은 코드를 작성하고, 운영팀은 그것을 인프라에 배포·관리하는 역할을 담당했다. 두 팀 사이에 담이 존재했고, 서로의 업무를 이해하지 못했다.

```
[전통적 개발 방식]

개발팀                          운영팀
┌──────────────┐               ┌──────────────┐
│  코드 작성   │               │ 서버 관리     │
│  기능 구현   │  "벽 너머로"  │ 배포 실행     │
│  버그 수정   │ ─────────→   │ 장애 대응     │
│  변경사항    │  코드 던지기  │ 안정성 유지   │
└──────────────┘               └──────────────┘
      ↑                               ↑
  빠른 변경 원함                안정성 원함
  (혁신 추구)                   (변화 두려움)
```

| 구분 | 전통적 개발 (Waterfall/Silo) | DevOps |
|------|------------------------------|--------|
| 배포 주기 | 수개월~1년 | 수시간~수일 (심지어 하루 수십 번) |
| 팀 구조 | 개발 vs 운영 분리 | 협업, 공동 책임 |
| 인프라 관리 | 수동 설정, 문서화 부족 | 코드로 관리 (IaC) |
| 장애 대응 | 범인 찾기 문화 | 비난 없는 포스트모텀 |
| 테스트 | 배포 직전 QA 단계 | 지속적 자동 테스트 |
| 피드백 주기 | 느림 (배포 후 수주) | 빠름 (배포 즉시) |
| 실패에 대한 태도 | 숨기고 회피 | 학습 기회로 활용 |

#### 왜 변화가 필요했나?

- **시장 속도**: 경쟁사가 주 단위로 기능을 출시하는데, 6개월에 한 번 배포하면 경쟁에서 뒤처진다.
- **고객 기대**: 사용자들은 버그 수정과 새 기능을 즉시 기대한다.
- **클라우드 등장**: AWS(2006), Azure(2010), GCP(2011)의 등장으로 인프라를 코드로 다룰 수 있게 됐다.
- **애자일의 한계**: 애자일로 개발 속도는 올렸지만, 운영팀이 따라가지 못하는 병목이 발생했다.

### 1.3 DevOps의 핵심 원칙 — CALMS 프레임워크

CALMS는 DevOps의 5가지 핵심 가치를 요약한 프레임워크다.

```
    C — Culture (문화)
    ├── 협업, 투명성, 공동 책임
    ├── 개발-운영 팀 간 신뢰 구축
    └── 실패를 학습으로 수용

    A — Automation (자동화)
    ├── 반복 작업 제거 (빌드, 테스트, 배포)
    ├── CI/CD 파이프라인 구축
    └── IaC (Infrastructure as Code)

    L — Lean (린)
    ├── 낭비 제거, 흐름 최적화
    ├── 작은 배치(batch) 단위 작업
    └── WIP(진행 중 작업) 제한

    M — Measurement (측정)
    ├── DORA 메트릭 추적
    ├── 데이터 기반 의사결정
    └── 지속적 개선 루프

    S — Sharing (공유)
    ├── 지식 공유, 문서화
    ├── 오픈소스 기여
    └── 팀 간 베스트 프랙티스 전파
```

| 원칙 | 핵심 질문 | 실천 방법 |
|------|-----------|-----------|
| Culture | "우리 팀이 함께 책임지는가?" | 공동 온콜, 팀 간 로테이션 |
| Automation | "이 작업을 사람이 반복해야 하는가?" | CI/CD, IaC, 자동화 테스트 |
| Lean | "낭비되는 단계가 있는가?" | 가치 흐름 매핑(VSM) |
| Measurement | "우리가 개선되고 있는지 어떻게 아는가?" | DORA 메트릭 대시보드 |
| Sharing | "배운 것을 팀 전체가 알고 있는가?" | 런북, 위키, 포스트모텀 |

### 1.4 DevOps vs Agile vs Waterfall 비교

```
[방법론 비교 다이어그램]

Waterfall (폭포수)
─────────────────────────────────────────────────────→ 시간
요구사항 → 설계 → 개발 → 테스트 → 배포 → 유지보수
(각 단계: 수개월, 역행 불가)

Agile (애자일)
Sprint 1     Sprint 2     Sprint 3     Sprint N
┌─────────┐  ┌─────────┐  ┌─────────┐
│계획/개발│→ │계획/개발│→ │계획/개발│→ ... → 배포
│테스트   │  │테스트   │  │테스트   │
└─────────┘  └─────────┘  └─────────┘
(2~4주 스프린트, 점진적 개선)

DevOps
┌──────────────────────────────────────────────────┐
│  Plan→Code→Build→Test→Release→Deploy→Operate→   │
│  ←──────────────── Monitor ─────────────────←   │
│            (지속적 무한루프)                      │
└──────────────────────────────────────────────────┘
(자동화 + 협업으로 하루에도 여러 번 배포 가능)
```

| 항목 | Waterfall | Agile | DevOps |
|------|-----------|-------|--------|
| 초점 | 프로세스 | 개발 속도 | 전체 가치 흐름 |
| 배포 주기 | 수개월~1년 | 스프린트 단위 | 지속적 (시간~일) |
| 피드백 루프 | 매우 느림 | 스프린트 후 | 즉각적 |
| 변경 적응 | 어려움 | 유연함 | 매우 유연함 |
| 팀 협업 | 사일로 | 개발팀 중심 | 개발+운영 통합 |
| 자동화 | 거의 없음 | 부분적 | 전면 자동화 |
| 문화 요소 | 계획 준수 | 반복 개선 | 공동 책임 |
| 관계 | 순차적 | 반복적 | 지속적+순환적 |

> 💡 **핵심**: DevOps는 Agile을 대체하는 것이 아니라 **확장**한다. Agile이 개발 팀 내 효율을 높인다면, DevOps는 개발→운영→피드백의 전체 사이클을 최적화한다.

---

## 2. DevOps 라이프사이클 (무한루프)

DevOps 라이프사이클은 끝이 없는 **∞ 무한루프(Infinite Loop)** 형태로 표현된다.

```
                    DevOps 무한루프 (Infinity Loop)

              PLAN ←──────────────────────── MONITOR
               │                                 ↑
               ↓                                 │
              CODE                           OPERATE
               │                                 ↑
               ↓                                 │
              BUILD ─────────────────────→  DEPLOY
               │                                 ↑
               ↓                                 │
              TEST ──────────────────────→ RELEASE
               │
               └──── (빠른 피드백) ─────────────→

    개발 단계 (Dev)          운영 단계 (Ops)
    ┌──────────────┐         ┌──────────────────┐
    │ Plan         │         │ Release          │
    │ Code         │         │ Deploy           │
    │ Build        │         │ Operate          │
    │ Test         │         │ Monitor          │
    └──────────────┘         └──────────────────┘
```

### 2.1 Plan (계획)

**목적**: 비즈니스 요구사항을 정의하고 작업을 우선순위화한다.

**주요 활동**:
- 백로그(Backlog) 관리
- 스프린트 계획
- 이슈 트래킹
- 로드맵 수립

**대표 도구**:

| 도구 | 특징 | 적합 규모 |
|------|------|-----------|
| Jira | 풍부한 기능, 커스터마이징 | 중~대규모 |
| GitHub Issues | 코드와 통합, 간편 | 소~중규모 |
| GitLab Issues | 올인원 (CI/CD 포함) | 소~대규모 |
| Trello | 칸반 보드, 직관적 | 소규모 |
| Linear | 빠르고 현대적인 UI | 스타트업 |

### 2.2 Code (코딩)

**목적**: 실제 소프트웨어 코드를 작성하고 버전을 관리한다.

**주요 활동**:
- 기능 개발 (Feature Branch)
- 코드 리뷰 (Pull Request / Merge Request)
- 페어 프로그래밍
- 코딩 표준 준수

**Git 기본 워크플로우**:

```bash
# 새 기능 브랜치 생성
git checkout -b feature/user-auth

# 변경사항 스테이징
git add .

# 커밋 (컨벤셔널 커밋 형식)
git commit -m "feat: add JWT authentication middleware"

# 원격 저장소에 푸시
git push origin feature/user-auth

# Pull Request 생성 후 코드 리뷰 → 메인 브랜치에 머지
```

**Git 브랜치 전략 비교**:

| 전략 | 구조 | 장점 | 단점 | 적합 환경 |
|------|------|------|------|-----------|
| Git Flow | main, develop, feature, release, hotfix | 릴리스 관리 체계적 | 복잡, 느린 배포 | 명확한 릴리스 주기 |
| GitHub Flow | main + feature branches | 단순, 빠른 배포 | 복수 버전 관리 어려움 | 지속적 배포 |
| GitLab Flow | main + environment branches | 환경별 관리 | 약간 복잡 | 스테이징 환경 필요 시 |
| Trunk-Based | main 하나 (단기 feature) | 매우 빠른 통합 | 피처 플래그 필수 | 고도화된 DevOps 팀 |

### 2.3 Build (빌드)

**목적**: 소스 코드를 실행 가능한 아티팩트(Artifact)로 변환한다.

**주요 활동**:
- 컴파일/패키징
- 의존성 해결
- 컨테이너 이미지 빌드
- 아티팩트 저장소 업로드

**빌드 도구 비교**:

| 언어/플랫폼 | 빌드 도구 |
|-------------|-----------|
| Java | Maven, Gradle |
| JavaScript/Node | npm, yarn, pnpm |
| Python | pip, Poetry, setuptools |
| Go | go build |
| 컨테이너 | Docker, Buildah, Kaniko |
| 멀티플랫폼 | Bazel, Make |

```bash
# Docker 이미지 빌드 예시
docker build -t myapp:1.0.0 .

# 멀티스테이지 빌드 (최적화된 이미지 크기)
# Dockerfile 예시
```

```dockerfile
# --- Build Stage ---
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

# --- Production Stage ---
FROM node:20-alpine AS production
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 3000
USER node
CMD ["node", "dist/server.js"]
```

### 2.4 Test (테스트)

**목적**: 코드 품질을 자동으로 검증하고 회귀를 방지한다.

**테스트 피라미드**:

```
                    ▲
                   /E2E\        ← 느림, 비용 높음, 적게
                  /─────\         (Selenium, Cypress, Playwright)
                 / Integr \
                /──────────\    ← 중간
               / Unit Tests \
              /──────────────\  ← 빠름, 저렴, 많이
             └────────────────┘   (Jest, JUnit, pytest)

  실행 속도: Unit > Integration > E2E
  유지 비용: Unit < Integration < E2E
  현실성:   Unit < Integration < E2E
```

| 테스트 유형 | 목적 | 도구 (예시) | 비율 권장 |
|------------|------|-------------|-----------|
| 단위(Unit) | 함수/클래스 단위 검증 | Jest, JUnit, pytest | ~70% |
| 통합(Integration) | 컴포넌트 간 상호작용 | Testcontainers, Postman | ~20% |
| E2E (End-to-End) | 실제 사용자 흐름 | Cypress, Playwright | ~10% |
| 성능(Performance) | 부하 및 응답 시간 | k6, JMeter, Locust | 별도 |
| 보안(Security) | 취약점 탐지 | OWASP ZAP, Trivy, Snyk | 별도 |

```yaml
# GitHub Actions에서 테스트 자동화 예시
- name: Run Unit Tests
  run: |
    npm test -- --coverage
    
- name: Upload Coverage
  uses: codecov/codecov-action@v3
  with:
    token: ${{ secrets.CODECOV_TOKEN }}
```

### 2.5 Release (릴리스)

**목적**: 검증된 빌드를 배포 가능한 상태로 준비한다.

**주요 활동**:
- 릴리스 버전 태깅 (Semantic Versioning)
- 변경 로그(Changelog) 생성
- 릴리스 노트 작성
- 환경별 설정 준비

**Semantic Versioning (SemVer)**:

```
MAJOR.MINOR.PATCH

예: 2.4.1
    │ │ └── PATCH: 버그 수정 (하위 호환)
    │ └──── MINOR: 기능 추가 (하위 호환)
    └────── MAJOR: 파괴적 변경 (하위 비호환)

pre-release: 1.0.0-alpha.1, 1.0.0-beta.2, 1.0.0-rc.1
```

### 2.6 Deploy (배포)

**목적**: 애플리케이션을 대상 환경에 실제로 배포한다.

**배포 환경 흐름**:

```
코드 → [Dev] → [Staging] → [Production]
         ↑          ↑            ↑
       개발자     QA팀       실제 사용자
       테스트     통합테스트   트래픽
```

**배포 전략** (자세한 내용은 6장 참고):
- Rolling Update
- Blue/Green Deployment
- Canary Release
- Feature Flag

### 2.7 Operate (운영)

**목적**: 운영 환경에서 서비스를 안정적으로 유지·관리한다.

**주요 활동**:
- 인프라 프로비저닝 및 구성 관리
- 확장(Scaling): 수평/수직 스케일링
- 온콜(On-Call) 대응
- 용량 계획(Capacity Planning)
- 비용 최적화

### 2.8 Monitor (모니터링)

**목적**: 시스템 상태와 비즈니스 메트릭을 실시간으로 추적하고, 이상 징후를 탐지한다.

**모니터링의 3가지 기둥 (Three Pillars of Observability)**:

```
┌─────────────────────────────────────────────────────┐
│              Observability (가시성)                  │
│                                                     │
│  ┌───────────┐  ┌───────────┐  ┌───────────────┐  │
│  │  Metrics  │  │   Logs    │  │   Traces      │  │
│  │ (메트릭)  │  │ (로그)    │  │ (분산 추적)   │  │
│  │           │  │           │  │               │  │
│  │ 숫자 기반  │  │ 이벤트    │  │ 요청 경로     │  │
│  │ 집계 데이터│  │ 텍스트    │  │ 서비스 간     │  │
│  │           │  │ 기록      │  │ 지연 추적     │  │
│  │Prometheus │  │ELK Stack  │  │Jaeger/Zipkin  │  │
│  │Grafana    │  │Loki       │  │OpenTelemetry  │  │
│  └───────────┘  └───────────┘  └───────────────┘  │
└─────────────────────────────────────────────────────┘
```

| 기둥 | 질문 답변 | 도구 |
|------|----------|------|
| Metrics | "시스템이 얼마나 잘 작동하는가?" | Prometheus, Grafana, Datadog |
| Logs | "무슨 일이 일어났는가?" | ELK Stack, Loki, Splunk |
| Traces | "어느 서비스에서 병목이 발생했는가?" | Jaeger, Zipkin, AWS X-Ray |

> 💡 **핵심 요약**: DevOps 라이프사이클은 8단계가 끊임없이 순환하며 각 단계에서 피드백이 이전 단계로 즉시 전달된다. 자동화가 핵심이며, 각 단계가 수동이면 DevOps가 아니다.

---

## 3. DevOps 핵심 실천 방법 (Practices)

### 3.1 CI/CD (지속적 통합/배포)

#### CI (Continuous Integration — 지속적 통합)

개발자가 코드를 **공유 저장소에 자주 병합**하고, 매번 자동 빌드·테스트를 실행하는 방식이다.

```
[CI 흐름]

개발자 A ─→ git push ─→ CI 서버 ─→ 빌드 ─→ 테스트 ─→ 결과 알림
개발자 B ─→ git push ─→              │           │
개발자 C ─→ git push ─→              ↓           ↓
                               성공: ✅        실패: ❌
                               다음 단계      개발자에게
                               진행           즉시 알림
```

**CI의 원칙**:
- 하루에 여러 번 메인 브랜치에 통합
- 빌드는 5~10분 이내 완료 (빠른 피드백)
- 빌드 실패 시 최우선으로 수정
- 테스트 커버리지 유지 (목표: 80%+)

#### CD (Continuous Delivery vs Continuous Deployment)

```
[CD의 두 가지 의미]

Continuous Delivery (지속적 전달)
─────────────────────────────────
CI → 빌드 → 테스트 → 스테이징 배포 → [수동 승인] → 프로덕션 배포
                                           ↑
                                      사람이 버튼 클릭

Continuous Deployment (지속적 배포)
────────────────────────────────────
CI → 빌드 → 테스트 → 스테이징 배포 → [자동 승인] → 프로덕션 배포
                                           ↑
                                      모든 게 통과하면 자동
```

| 구분 | Continuous Delivery | Continuous Deployment |
|------|--------------------|-----------------------|
| 자동화 범위 | 스테이징까지 자동 | 프로덕션까지 완전 자동 |
| 승인 | 수동 게이트 존재 | 완전 자동화 |
| 위험도 | 낮음 | 높음 (하지만 빠름) |
| 요구 조건 | 좋은 테스트 커버리지 | 매우 높은 테스트 신뢰도 |
| 적합 환경 | 규제 산업, 엔터프라이즈 | SaaS, 웹 서비스 |

**GitHub Actions CI/CD 파이프라인 전체 예시**:

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  IMAGE_NAME: myapp
  REGISTRY: ghcr.io/${{ github.repository_owner }}

jobs:
  # ─── 1단계: 코드 품질 검사 ───
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run linter
        run: npm run lint
      
      - name: Run unit tests
        run: npm test -- --coverage
      
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3

  # ─── 2단계: 보안 스캔 ───
  security-scan:
    runs-on: ubuntu-latest
    needs: lint-and-test
    steps:
      - uses: actions/checkout@v4
      
      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          exit-code: '1'
          severity: 'CRITICAL,HIGH'

  # ─── 3단계: Docker 이미지 빌드 & 푸시 ───
  build-and-push:
    runs-on: ubuntu-latest
    needs: [lint-and-test, security-scan]
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      
      - name: Login to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Build and push image
        uses: docker/build-push-action@v5
        with:
          push: true
          tags: |
            ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:latest
            ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}

  # ─── 4단계: 스테이징 배포 ───
  deploy-staging:
    runs-on: ubuntu-latest
    needs: build-and-push
    environment: staging
    steps:
      - name: Deploy to staging
        run: |
          echo "Deploying ${{ github.sha }} to staging..."
          # kubectl set image deployment/myapp myapp=${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}

  # ─── 5단계: 프로덕션 배포 (수동 승인) ───
  deploy-production:
    runs-on: ubuntu-latest
    needs: deploy-staging
    environment: production   # GitHub 환경 보호 규칙으로 수동 승인
    steps:
      - name: Deploy to production
        run: |
          echo "Deploying ${{ github.sha }} to production..."
```

### 3.2 IaC (Infrastructure as Code)

**IaC**는 인프라를 코드로 정의하고 관리하는 접근 방식이다. 서버, 네트워크, 데이터베이스 등 모든 인프라를 코드 파일로 선언하고, 버전을 관리하며, 자동으로 프로비저닝한다.

**IaC의 이점**:

```
[IaC 없을 때 vs IaC 있을 때]

IaC 없음 (수동 관리)              IaC 있음
─────────────────────────        ─────────────────────────
- SSH로 접속 후 수동 설정         - 코드 파일 작성
- 설정이 팀원마다 다름            - git commit → apply
- 재현 불가능한 환경              - 완전 동일한 환경 재현
- "내 로컬에선 됐는데..."         - 모든 환경 일관성 보장
- 문서화 안 된 설정              - 코드가 곧 문서
- 드리프트(Drift) 발생           - 드리프트 감지 및 수정
```

**Terraform 기본 예시 (AWS EC2 프로비저닝)**:

```hcl
# main.tf — AWS EC2 인스턴스 + 보안 그룹 생성

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  # 원격 상태 저장 (팀 협업 필수)
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/main.tfstate"
    region = "ap-northeast-2"
  }
}

provider "aws" {
  region = var.aws_region
}

# 변수 정의
variable "aws_region" {
  default = "ap-northeast-2"
}

variable "instance_type" {
  default = "t3.micro"
}

variable "environment" {
  type = string
}

# 보안 그룹
resource "aws_security_group" "web" {
  name        = "${var.environment}-web-sg"
  description = "Web server security group"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}

# EC2 인스턴스
resource "aws_instance" "web" {
  ami           = data.aws_ami.amazon_linux.id
  instance_type = var.instance_type
  
  vpc_security_group_ids = [aws_security_group.web.id]
  
  user_data = <<-EOF
    #!/bin/bash
    yum update -y
    yum install -y docker
    systemctl start docker
    systemctl enable docker
  EOF

  tags = {
    Name        = "${var.environment}-web-server"
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}

# 최신 Amazon Linux 2 AMI 조회
data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}

# 출력값
output "instance_public_ip" {
  value       = aws_instance.web.public_ip
  description = "EC2 인스턴스 퍼블릭 IP"
}
```

```bash
# Terraform 기본 명령어
terraform init      # 플러그인 초기화
terraform plan      # 변경사항 미리 보기 (Dry Run)
terraform apply     # 실제 적용
terraform destroy   # 리소스 삭제
terraform fmt       # 코드 포맷팅
terraform validate  # 구문 유효성 검사
terraform state list # 상태 파일의 리소스 목록
```

**IaC 도구 비교**:

| 도구 | 언어 | 상태 관리 | 멀티클라우드 | 특징 |
|------|------|-----------|--------------|------|
| Terraform | HCL | 상태 파일 (로컬/원격) | ✅ 우수 | 가장 널리 사용, 생태계 풍부 |
| CloudFormation | JSON/YAML | AWS 관리 | ❌ AWS 전용 | AWS 네이티브, 무료 |
| Pulumi | Python/TS/Go | 상태 파일 | ✅ | 일반 프로그래밍 언어 사용 |
| Ansible | YAML | 상태 없음(Agentless) | ✅ | 설정 관리 + 간단한 IaC |
| CDK | TypeScript/Python | CloudFormation | ❌ AWS 전용 | 코드로 CloudFormation 생성 |

### 3.3 구성 관리 (Configuration Management)

**구성 관리**는 서버의 소프트웨어 설정, 패키지, 서비스 상태 등을 코드로 정의하고 일관성 있게 적용하는 것이다. IaC가 인프라 *생성*에 집중한다면, 구성 관리는 생성된 인프라의 *설정*에 집중한다.

**Ansible 기본 예시**:

```yaml
# playbook.yml — 웹 서버 설정 자동화

---
- name: Configure Web Servers
  hosts: webservers
  become: yes  # sudo 권한 사용
  vars:
    app_port: 8080
    app_user: appuser
    
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes
        cache_valid_time: 3600

    - name: Install required packages
      apt:
        name:
          - nginx
          - git
          - curl
        state: present

    - name: Create application user
      user:
        name: "{{ app_user }}"
        system: yes
        shell: /bin/bash
        create_home: yes

    - name: Copy nginx configuration
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/myapp
        owner: root
        group: root
        mode: '0644'
      notify: Reload nginx

    - name: Enable nginx site
      file:
        src: /etc/nginx/sites-available/myapp
        dest: /etc/nginx/sites-enabled/myapp
        state: link

    - name: Start and enable nginx
      service:
        name: nginx
        state: started
        enabled: yes

  handlers:
    - name: Reload nginx
      service:
        name: nginx
        state: reloaded
```

```ini
# inventory.ini — 인벤토리 파일 (관리 대상 서버 목록)
[webservers]
web1.example.com ansible_user=ubuntu
web2.example.com ansible_user=ubuntu

[dbservers]
db1.example.com ansible_user=ubuntu

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

```bash
# Ansible 기본 명령어
ansible all -m ping                        # 연결 테스트
ansible webservers -m command -a "uptime"  # 임시 명령 실행
ansible-playbook playbook.yml              # 플레이북 실행
ansible-playbook playbook.yml --check      # Dry Run (변경 없이 미리 보기)
ansible-playbook playbook.yml -l web1      # 특정 호스트만 실행
ansible-vault encrypt secrets.yml         # 시크릿 암호화
```

**구성 관리 도구 비교**:

| 도구 | 방식 | 에이전트 | 언어 | 학습 곡선 | 특징 |
|------|------|----------|------|-----------|------|
| Ansible | Push (SSH) | 없음 | YAML | 낮음 | 가장 쉬움, 빠른 시작 |
| Chef | Pull | 필요 | Ruby DSL | 높음 | 유연, 코드 중심 |
| Puppet | Pull | 필요 | DSL/Ruby | 높음 | 엔터프라이즈, 성숙 |
| SaltStack | Push/Pull | 선택적 | Python/YAML | 중간 | 빠른 실행 |

### 3.4 컨테이너화 (Containerization)

**컨테이너**는 애플리케이션과 그 실행에 필요한 모든 의존성(라이브러리, 런타임, 설정)을 하나의 격리된 패키지로 묶는 기술이다.

**VM vs 컨테이너 비교**:

```
[가상 머신 (VM)]                    [컨테이너]
┌────────────────────────────┐      ┌────────────────────────────┐
│  App A   │  App B          │      │  App A   │  App B          │
├──────────┼──────────────── │      ├──────────┼──────────────── │
│  Guest   │  Guest OS       │      │  Container│  Container     │
│  OS      │                 │      │  Runtime  │  Runtime       │
├──────────┴─────────────────┤      ├──────────────────────────  │
│       Hypervisor           │      │      Docker Engine          │
├────────────────────────────┤      ├────────────────────────────┤
│         Host OS            │      │         Host OS             │
├────────────────────────────┤      ├────────────────────────────┤
│         Hardware           │      │         Hardware            │
└────────────────────────────┘      └────────────────────────────┘

크기: GB 단위                        크기: MB 단위
기동 시간: 수분                      기동 시간: 수초
격리: 완전한 OS 격리                 격리: 프로세스 수준
```

| 비교 항목 | VM | 컨테이너 |
|----------|-----|---------|
| 크기 | GB | MB |
| 시작 시간 | 분 단위 | 초 단위 |
| OS | 각자 독립 | 호스트 커널 공유 |
| 격리 수준 | 강함 | 약함 (하지만 충분) |
| 오버헤드 | 높음 | 낮음 |
| 이식성 | 낮음 | 높음 |
| 적합 용도 | 레거시 앱, 강한 격리 필요 | 마이크로서비스, CI/CD |

**Docker 핵심 명령어**:

```bash
# 이미지 관리
docker pull nginx:alpine              # 이미지 다운로드
docker images                         # 이미지 목록
docker build -t myapp:1.0 .           # 이미지 빌드
docker push myregistry/myapp:1.0      # 이미지 푸시
docker rmi myapp:1.0                  # 이미지 삭제

# 컨테이너 실행
docker run -d \                        # 백그라운드 실행
  --name web \                         # 컨테이너 이름
  -p 8080:80 \                         # 포트 매핑 (호스트:컨테이너)
  -e ENV=production \                  # 환경 변수
  -v /data:/app/data \                 # 볼륨 마운트
  --restart always \                   # 재시작 정책
  nginx:alpine

# 컨테이너 관리
docker ps                              # 실행 중인 컨테이너
docker ps -a                           # 모든 컨테이너
docker logs -f web                     # 로그 실시간 확인
docker exec -it web /bin/sh            # 컨테이너 쉘 접속
docker stop web && docker rm web       # 중지 및 삭제
docker stats                           # 리소스 사용량 모니터링

# Docker Compose (다중 컨테이너 관리)
docker compose up -d                   # 서비스 시작
docker compose down                    # 서비스 중지 및 네트워크 제거
docker compose logs -f                 # 모든 서비스 로그
docker compose ps                      # 서비스 상태
```

**Docker Compose 예시 (웹앱 + DB + 캐시)**:

```yaml
# docker-compose.yml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
      - REDIS_URL=redis://cache:6379
    depends_on:
      db:
        condition: service_healthy
      cache:
        condition: service_started
    restart: unless-stopped
    
  db:
    image: postgres:16-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 10s
      timeout: 5s
      retries: 5
      
  cache:
    image: redis:7-alpine
    volumes:
      - redis_data:/data
    command: redis-server --appendonly yes

volumes:
  postgres_data:
  redis_data:
```

### 3.5 오케스트레이션 (Orchestration)

컨테이너 수가 많아지면 개별 관리는 불가능하다. **컨테이너 오케스트레이션**은 수십~수천 개의 컨테이너를 자동으로 배포, 스케일링, 장애 복구, 로드 밸런싱하는 시스템이다.

```
[Kubernetes 아키텍처]

┌────────────────────────────────────────────────────────────┐
│                   Control Plane (마스터)                    │
│  ┌────────────┐  ┌───────────┐  ┌────────────────────────┐│
│  │ API Server │  │ etcd      │  │ Controller Manager     ││
│  │ (진입점)   │  │ (상태 저장)│  │ (원하는 상태 유지)     ││
│  └────────────┘  └───────────┘  └────────────────────────┘│
│  ┌─────────────────────────────────────────────────────┐  │
│  │                    Scheduler                         │  │
│  │              (파드를 노드에 배치)                    │  │
│  └─────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────┘
         │                 │                 │
         ↓                 ↓                 ↓
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   Worker     │  │   Worker     │  │   Worker     │
│   Node 1     │  │   Node 2     │  │   Node 3     │
│  ┌────────┐  │  │  ┌────────┐  │  │  ┌────────┐  │
│  │ kubelet│  │  │  │ kubelet│  │  │  │ kubelet│  │
│  ├────────┤  │  │  ├────────┤  │  │  ├────────┤  │
│  │kube-   │  │  │  │kube-   │  │  │  │kube-   │  │
│  │proxy   │  │  │  │proxy   │  │  │  │proxy   │  │
│  ├────────┤  │  │  ├────────┤  │  │  ├────────┤  │
│  │ Pod    │  │  │  │ Pod    │  │  │  │ Pod    │  │
│  │ Pod    │  │  │  │ Pod    │  │  │  │ Pod    │  │
│  └────────┘  │  │  └────────┘  │  │  └────────┘  │
└──────────────┘  └──────────────┘  └──────────────┘
```

**핵심 Kubernetes 오브젝트**:

```yaml
# deployment.yaml — 애플리케이션 배포 정의
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  template:
    metadata:
      labels:
        app: myapp
        version: "1.0.0"
    spec:
      containers:
      - name: myapp
        image: myregistry/myapp:1.0.0
        ports:
        - containerPort: 3000
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
        livenessProbe:         # 살아있는지 확인
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:        # 트래픽 받을 준비 됐는지 확인
          httpGet:
            path: /ready
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: url
---
# service.yaml — 네트워크 접근 설정
apiVersion: v1
kind: Service
metadata:
  name: myapp-svc
spec:
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 3000
  type: ClusterIP
```

```bash
# kubectl 핵심 명령어
kubectl get pods -n production         # 파드 목록
kubectl describe pod myapp-xxx         # 파드 상세 정보
kubectl logs -f myapp-xxx              # 로그 확인
kubectl exec -it myapp-xxx -- /bin/sh  # 파드 쉘 접속
kubectl apply -f deployment.yaml       # 리소스 적용
kubectl delete -f deployment.yaml      # 리소스 삭제
kubectl scale deployment myapp --replicas=5  # 스케일 조정
kubectl rollout status deployment/myapp      # 배포 상태 확인
kubectl rollout undo deployment/myapp        # 이전 버전으로 롤백
kubectl top pods                             # 리소스 사용량
```

### 3.6 GitOps

**GitOps**는 Git 저장소를 **단일 진실 공급원(Single Source of Truth)**으로 사용하여 인프라와 애플리케이션의 원하는 상태를 선언적으로 관리하는 운영 방식이다.

```
[GitOps 흐름]

개발자
  │
  ↓ git push (원하는 상태 정의)
Git 저장소 (Kubernetes YAML 등)
  │
  ↓ 자동 감지 (Pull 방식)
GitOps 에이전트 (ArgoCD / Flux)
  │
  ↓ 클러스터 상태 비교 (Desired vs Actual)
Kubernetes 클러스터
  │
  ↓ 차이가 있으면 자동 동기화
실제 운영 상태

[핵심 원칙]
1. Git이 모든 것의 소스
2. 선언적(Declarative) 설정
3. 자동 동기화 (Pull 기반)
4. 감사 추적 (Git 히스토리)
```

**GitOps의 장점**:
- 모든 변경이 Git 커밋으로 기록됨 (감사 추적)
- Pull Request를 통한 변경 리뷰
- 클러스터 자동 복구 (드리프트 감지)
- 여러 환경 관리 용이

### 3.7 DevSecOps (보안 통합)

**DevSecOps**는 DevOps 파이프라인에 보안을 처음부터 통합하는 접근 방식이다. "보안은 나중에"가 아닌 **"보안을 왼쪽으로 이동(Shift Left)"**이 핵심이다.

```
[Shift Left Security]

왼쪽 (코딩 단계)                    오른쪽 (운영 단계)
     ↑                                    ↑
  저비용                               고비용
  빠른 수정                            느린 수정

Plan → Code → Build → Test → Release → Deploy → Operate → Monitor
  │       │      │       │       │         │         │
  SAST    SCA   DAST   컨테이너  서명     RBAC    런타임
 (정적  (의존성  (동적   보안     검증    정책    보안
  분석)  스캔)  분석)   스캔             
```

| 단계 | 보안 활동 | 도구 |
|------|----------|------|
| Code | SAST (정적 분석), 시크릿 스캔 | SonarQube, GitLeaks, Semgrep |
| Build | 의존성 취약점 스캔 (SCA) | Snyk, OWASP Dependency-Check |
| 컨테이너 | 이미지 취약점 스캔 | Trivy, Clair, Anchore |
| Deploy | 컨테이너 서명 검증 | Cosign, Notary |
| Operate | RBAC, 네트워크 정책 | Kubernetes RBAC, OPA |
| Monitor | 런타임 위협 탐지 | Falco, Sysdig |

> 💡 **핵심 요약**: DevOps 실천 방법은 서로 독립적이지 않다. CI/CD → 컨테이너 → Kubernetes → GitOps → IaC → 보안이 층층이 연결되어 완성된 DevOps 파이프라인을 이룬다.

---

## 4. DevOps 도구 생태계

DevOps 생태계는 수백 가지 도구로 구성된다. 카테고리별로 주요 도구를 비교한다.

```
[DevOps 도구 맵]

┌─────────────────────────────────────────────────────────────┐
│                    DevOps 도구 생태계                        │
│                                                             │
│  Plan          Code           Build          Test           │
│  ┌────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │Jira    │  │Git       │  │Maven     │  │Jest      │    │
│  │GitHub  │  │GitHub    │  │Gradle    │  │JUnit     │    │
│  │Issues  │  │GitLab    │  │Docker    │  │Selenium  │    │
│  └────────┘  └──────────┘  └──────────┘  └──────────┘    │
│                                                             │
│  Release       Deploy         Operate        Monitor        │
│  ┌────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │GitHub  │  │ArgoCD    │  │K8s       │  │Prometheus│    │
│  │Actions │  │Flux      │  │Ansible   │  │Grafana   │    │
│  │Jenkins │  │Helm      │  │Terraform │  │ELK Stack │    │
│  └────────┘  └──────────┘  └──────────┘  └──────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### 4.1 버전 관리

| 도구 | 유형 | 특징 | 적합 용도 |
|------|------|------|-----------|
| Git | 로컬 VCS | 분산형, 빠름 | 모든 프로젝트 |
| GitHub | 호스팅 서비스 | 오픈소스 생태계, GitHub Actions | 오픈소스, 스타트업 |
| GitLab | 올인원 플랫폼 | CI/CD 내장, 셀프 호스팅 가능 | 엔터프라이즈, DevOps 통합 |
| Bitbucket | 호스팅 서비스 | Jira/Confluence 연동 | Atlassian 생태계 |
| AWS CodeCommit | 관리형 Git | AWS 통합 | AWS 중심 팀 |

### 4.2 CI/CD

| 도구 | 유형 | 장점 | 단점 | 학습 곡선 |
|------|------|------|------|-----------|
| GitHub Actions | 클라우드 | GitHub 통합, 마켓플레이스 풍부 | GitHub 의존 | 낮음 |
| GitLab CI | 클라우드/셀프 | 올인원, GitOps 지원 | 복잡한 설정 | 중간 |
| Jenkins | 셀프 호스팅 | 최강 유연성, 플러그인 풍부 | 유지보수 부담 | 높음 |
| CircleCI | 클라우드 | 빠른 빌드, 병렬화 | 비용 | 중간 |
| ArgoCD | CD 전문 (K8s) | GitOps 최적화 | K8s 전용 | 중간 |
| Tekton | K8s 네이티브 CI | 클라우드 네이티브 | 복잡 | 높음 |

**Jenkins vs GitHub Actions 비교**:

```
Jenkins                              GitHub Actions
─────────────────────────────────    ─────────────────────────────────
✅ 완전한 커스터마이징               ✅ GitHub와 원활한 통합
✅ 수천 개 플러그인                  ✅ 마켓플레이스 (Actions)
✅ 복잡한 파이프라인 지원            ✅ 관리 부담 없음 (클라우드)
✅ 셀프 호스팅 (보안 환경)           ✅ 간단한 YAML 문법
❌ 설치·유지보수 부담                ✅ 무료 공개 저장소
❌ Groovy DSL 학습 필요              ❌ GitHub 의존성
❌ 오래된 UI                         ❌ 셀프 호스팅 러너 설정 필요
```

### 4.3 컨테이너

| 도구 | 특징 | 사용 시점 |
|------|------|-----------|
| Docker | 사실상 표준, 개발 친화적 | 개발 환경, 단일 서버 |
| Podman | 루트리스(rootless), Docker 호환 | 보안 중요 환경, RHEL |
| containerd | 경량, K8s 기본 런타임 | Kubernetes 운영 |
| nerdctl | containerd용 Docker 호환 CLI | containerd 사용 시 |
| Buildah | OCI 이미지 빌드 전용 | CI 환경에서 이미지 빌드 |
| Kaniko | 데몬 없이 빌드 (K8s 내) | Kubernetes CI/CD |

### 4.4 오케스트레이션

| 도구 | 복잡도 | 기능 | 적합 규모 |
|------|--------|------|-----------|
| Kubernetes (K8s) | 높음 | 완전한 기능 | 중~대규모 |
| K3s | 낮음 | K8s 경량화 | 엣지, 소규모 |
| Docker Swarm | 낮음 | 간단한 오케스트레이션 | 소규모 (레거시) |
| Amazon ECS | 중간 | AWS 관리형 | AWS 사용 팀 |
| Nomad | 중간 | 컨테이너+VM+바이너리 | 멀티 워크로드 |

### 4.5 IaC

| 도구 | 언어 | 특징 |
|------|------|------|
| Terraform | HCL | 멀티클라우드 표준, 거대한 모듈 생태계 |
| OpenTofu | HCL | Terraform 오픈소스 포크 (MPL 라이선스) |
| AWS CDK | TS/Python/Java | CloudFormation 코드 추상화 |
| Pulumi | TS/Python/Go | 실제 프로그래밍 언어 사용 |
| Crossplane | YAML | K8s 방식으로 클라우드 리소스 관리 |

### 4.6 구성 관리

| 도구 | 아키텍처 | 언어 | 에이전트 | 특징 |
|------|----------|------|----------|------|
| Ansible | Push | YAML | 없음 | 가장 쉬움, SSH 기반 |
| Chef | Pull | Ruby | 필요 | 강력, 코드 중심 |
| Puppet | Pull | DSL | 필요 | 성숙, 엔터프라이즈 |
| SaltStack | Push/Pull | Python/YAML | 선택 | 빠른 실행 속도 |

### 4.7 모니터링 & 알림

| 도구 | 유형 | 특징 | 비용 |
|------|------|------|------|
| Prometheus | 메트릭 수집 | K8s 표준, 강력한 쿼리 언어 PromQL | 오픈소스 |
| Grafana | 시각화 | 멀티 소스 대시보드, 알림 | 오픈소스 |
| Alertmanager | 알림 | Prometheus 연동, 알림 라우팅 | 오픈소스 |
| Datadog | APM + 모든 것 | 올인원, 쉬운 설정 | 비쌈 |
| New Relic | APM | 풀스택 관측성 | 유료 |
| Victoria Metrics | 메트릭 | Prometheus 호환, 고성능 | 오픈소스 |

**Prometheus + Grafana 스택 구성**:

```yaml
# prometheus.yml — Prometheus 기본 설정
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  - "alert_rules.yml"

alerting:
  alertmanagers:
    - static_configs:
        - targets:
          - alertmanager:9093

scrape_configs:
  - job_name: 'kubernetes-pods'
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        regex: true

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

```yaml
# alert_rules.yml — 알림 규칙
groups:
  - name: application
    rules:
      - alert: HighErrorRate
        expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.05
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "High error rate detected"
          description: "Error rate is {{ $value | humanizePercentage }}"

      - alert: PodCrashLooping
        expr: kube_pod_container_status_restarts_total > 5
        for: 10m
        labels:
          severity: warning
```

### 4.8 로깅

| 도구 | 유형 | 특징 |
|------|------|------|
| Elasticsearch | 저장/검색 | 강력한 전문 검색, 스케일 우수 |
| Logstash | 수집/파싱 | 풍부한 필터 플러그인 |
| Kibana | 시각화 | ES 기본 대시보드 |
| Filebeat | 경량 수집기 | 낮은 리소스 사용 |
| Loki | 저장 | Grafana 통합, 저비용 (인덱싱 최소화) |
| Promtail | 수집기 | Loki 전용 |
| Fluentd | 수집/파싱 | CNCF 프로젝트, K8s 표준 |

**ELK vs Loki 비교**:

```
ELK Stack                          Grafana Loki
─────────────────────────────────  ─────────────────────────────────
✅ 강력한 전문 검색                 ✅ 저비용 (인덱싱 최소화)
✅ 풍부한 기능                     ✅ Grafana와 완벽 통합
✅ 성숙한 생태계                   ✅ Prometheus 유사 쿼리 (LogQL)
❌ 높은 리소스 사용                ✅ K8s 네이티브
❌ 복잡한 운영                     ❌ 검색 기능 상대적 제한
❌ 높은 비용                       ❌ ELK보다 젊은 생태계
```

### 4.9 클라우드 플랫폼 비교

| 항목 | AWS | Azure | GCP |
|------|-----|-------|-----|
| 시장 점유율 | 1위 (~32%) | 2위 (~23%) | 3위 (~12%) |
| 설립 | 2006 | 2010 | 2011 |
| 강점 | 서비스 다양성, 성숙도 | Microsoft 통합, 엔터프라이즈 | AI/ML, Kubernetes (GKE) |
| 컨테이너 서비스 | ECS, EKS, Fargate | AKS | GKE |
| IaC 도구 | CloudFormation, CDK | ARM Templates, Bicep | Deployment Manager |
| 서버리스 | Lambda | Azure Functions | Cloud Functions |
| 관리형 DB | RDS, Aurora, DynamoDB | Azure SQL, Cosmos DB | Cloud SQL, Spanner |
| 한국 리전 | 서울 (ap-northeast-2) | 한국 중부 | 서울 (asia-northeast3) |
| 자격증 | AWS SAA/SAP, CDA 등 | AZ-900, AZ-104 등 | Associate Cloud Engineer 등 |

### 4.10 도구 선택 기준

```
도구 선택 시 고려 요소

1. 팀 규모와 기술 수준
   └── 소규모팀 → Ansible + GitHub Actions (단순)
   └── 대규모팀 → Terraform + Jenkins + ArgoCD (강력)

2. 클라우드 환경
   └── AWS 중심 → CloudFormation + CodePipeline
   └── 멀티클라우드 → Terraform + GitHub Actions

3. 운영 비용 (오픈소스 vs 관리형)
   └── 비용 민감 → 오픈소스 (Prometheus, Loki, K8s)
   └── 관리 부담 최소화 → Datadog, New Relic, EKS

4. 레거시 시스템 연동
   └── 온프레미스 많음 → Ansible + Jenkins
   └── 클라우드 네이티브 → Kubernetes + GitOps

5. 컴플라이언스 & 보안
   └── 규제 산업 → 셀프 호스팅 (Jenkins, GitLab CE)
   └── 일반 → 클라우드 관리형
```

> 💡 **핵심 요약**: 도구 선택보다 **문화와 프로세스**가 먼저다. 완벽한 도구 세트가 아니라 팀에 맞는 도구를 선택하고 점진적으로 개선한다. "도구의 수를 최소화"하는 것이 운영 복잡도를 낮추는 길이다.

---

## 5. DevOps 문화와 조직

### 5.1 DevOps 문화의 핵심

DevOps는 기술보다 **문화**가 더 중요하다. 최고의 도구를 갖춰도 문화가 없으면 실패한다.

```
[DevOps 문화의 4가지 요소]

      협업 (Collaboration)
      ┌──────────────────────┐
      │  개발 + 운영 공동책임 │
      │  팀 간 투명한 소통    │
      └──────────────────────┘
              ↕
자동화 ◄──────────────────► 피드백 루프
(Automation)               (Fast Feedback)
      ┌────────┐   ┌────────────────────┐
      │반복 작업│   │실시간 메트릭 공유  │
      │제거    │   │빠른 실패·빠른 수정 │
      └────────┘   └────────────────────┘
              ↕
      지속적 학습 (Continuous Learning)
      ┌──────────────────────┐
      │  실험 장려            │
      │  실패에서 학습        │
      └──────────────────────┘
```

**심리적 안전감(Psychological Safety)**이 DevOps 문화의 토대다. Google의 Project Aristotle 연구에서 팀 효과성의 가장 중요한 요소로 밝혀졌다. 팀원이 실수를 두려워하지 않고, 아이디어를 자유롭게 제안할 수 있어야 DevOps가 작동한다.

### 5.2 SRE (Site Reliability Engineering) vs DevOps

**SRE**는 Google이 2003년에 창안한 개념으로, 소프트웨어 엔지니어링 원칙을 운영에 적용하는 역할이다.

```
[DevOps vs SRE 관계]

DevOps ─── 철학/문화/방법론 ("무엇을")
  │
  └── SRE ─── 구체적 구현 방식 ("어떻게")
               (Google의 DevOps 구현체)
```

| 비교 항목 | DevOps | SRE |
|----------|--------|-----|
| 본질 | 문화·철학 | 역할·직무 |
| 목표 | 개발-운영 협업 | 시스템 신뢰성 |
| 접근법 | 협업, 자동화 | 엔지니어링으로 운영 해결 |
| 주요 지표 | DORA 메트릭 | SLA/SLO/SLI, 에러 버짓 |
| 토보라이트 | 없음 | Toil 감소 목표 |
| 팀 구성 | 개발+운영 통합 | 전문 SRE 팀 |
| 출처 | 커뮤니티 | Google |

**SRE 핵심 개념**:

- **Toil**: 수동적이고 반복적이며, 장기적 가치 없이 서비스를 유지하는 작업. SRE는 Toil을 50% 이하로 유지해야 한다.
- **Error Budget**: SLO가 99.9%라면, 0.1%의 실패 허용 예산. 에러 버짓이 남아있으면 새 기능 배포 가능, 소진되면 안정화 우선.
- **Blameless Postmortem**: 장애 분석 시 개인 비난 없이 시스템 문제에 집중.

### 5.3 Platform Engineering

**Platform Engineering**은 내부 개발 팀을 위한 셀프 서비스 플랫폼(Internal Developer Platform, IDP)을 구축하는 새로운 분야다.

```
[Platform Engineering 개념]

                 개발 팀 (소비자)
          ┌────────────────────────┐
          │  코드 작성에만 집중    │
          │  인프라 신경 안 써도 됨│
          └────────────┬───────────┘
                       │ 셀프 서비스
                       ↓
          ┌────────────────────────┐
          │  Internal Developer   │
          │  Platform (IDP)       │
          │  ┌─────────────────┐  │
          │  │ 배포 버튼 클릭  │  │
          │  │ 환경 자동 생성  │  │
          │  │ 모니터링 자동   │  │
          │  └─────────────────┘  │
          └────────────┬───────────┘
                       │ 관리
                       ↓
          ┌────────────────────────┐
          │  Platform Team        │
          │  (플랫폼 팀)          │
          └────────────────────────┘
```

**Platform Engineering vs DevOps**:
- DevOps: "You build it, you run it" (개발팀이 운영까지)
- Platform Engineering: 플랫폼 팀이 공통 인프라 제공 → 개발팀은 비즈니스 로직에 집중

**주요 도구**: Backstage (Spotify), Port, Cortex

### 5.4 DevOps 팀 구조 모델

| 모델 | 설명 | 장점 | 단점 |
|------|------|------|------|
| Embedded DevOps | DevOps 엔지니어가 각 개발팀에 배치 | 팀 맞춤형, 긴밀한 협업 | 일관성 부족, 중복 |
| Platform Team | 중앙 플랫폼 팀 + 개발팀 | 표준화, 효율적 | 병목 발생 가능 |
| CoE (Center of Excellence) | 중앙 DevOps 전문가 그룹 | 지식 집중 | 의존성, 병목 |
| 완전 통합 | 구분 없이 모두가 DevOps | 자율성 높음 | 역량 편차 |

### 5.5 Blameless Postmortem (비난 없는 포스트모텀)

장애 발생 후 **개인 비난 없이** 시스템과 프로세스 관점에서 원인을 분석하고 재발 방지책을 마련하는 문화다.

**포스트모텀 구조**:

```markdown
## 장애 포스트모텀: [서비스명] DB 연결 실패
**날짜**: 2025-03-15  
**영향**: API 응답 100% 실패 (22:30~23:15, 45분)  
**작성자**: [팀원 A, B, C]

### 1. 타임라인
- 22:30 — 모니터링 알림 발생 (에러율 100%)
- 22:33 — 온콜 담당자 인지
- 22:45 — 원인 파악 (DB 연결 풀 소진)
- 23:00 — 임시 조치 (연결 풀 크기 증가)
- 23:15 — 서비스 정상화

### 2. 근본 원인 (Root Cause)
신규 기능 배포 시 N+1 쿼리 문제로 DB 연결 급증

### 3. 기여 요인
- 스테이징 환경에서 부하 테스트 미실시
- DB 연결 풀 모니터링 알림 임계값 부재

### 4. 재발 방지 액션
| 액션 | 담당자 | 기한 |
|------|--------|------|
| 성능 테스트 CI 단계 추가 | 개발팀 | 2주 |
| DB 연결 풀 알림 설정 | 운영팀 | 3일 |
| 쿼리 성능 리뷰 프로세스 수립 | 팀장 | 1주 |

### 5. 칭찬할 점 (What Went Well)
- 빠른 인지 및 대응 (3분)
- 팀 협업으로 신속한 원인 파악
```

> 💡 **핵심 요약**: DevOps 문화는 도구보다 중요하다. 심리적 안전감, 협업, 빠른 피드백, 지속적 학습이 기반이다. SRE는 DevOps의 구체적 구현이며, Platform Engineering은 DevOps의 진화된 형태다.

---

## 6. DevOps 핵심 개념 심화

### 6.1 마이크로서비스 아키텍처 vs 모놀리식

```
[모놀리식 아키텍처]

┌──────────────────────────────────────┐
│           단일 애플리케이션            │
│  ┌────────┐ ┌────────┐ ┌──────────┐  │
│  │ 사용자 │ │ 주문   │ │ 상품     │  │
│  │ 관리   │ │ 처리   │ │ 관리     │  │
│  └────────┘ └────────┘ └──────────┘  │
│  ┌────────┐ ┌────────┐ ┌──────────┐  │
│  │ 결제   │ │ 알림   │ │ 배송     │  │
│  │ 처리   │ │ 서비스 │ │ 추적     │  │
│  └────────┘ └────────┘ └──────────┘  │
└──────────────────────────────────────┘
              ↓ 단일 DB
         ┌─────────┐
         │Database │
         └─────────┘

[마이크로서비스 아키텍처]

┌──────────┐  ┌──────────┐  ┌──────────┐
│사용자 서비│  │주문 서비스│  │상품 서비스│
│스        │  │          │  │          │
│DB: Users │  │DB: Orders│  │DB: Items │
└────┬─────┘  └────┬─────┘  └────┬─────┘
     │              │              │
     └──────────────┼──────────────┘
                    │ API Gateway / Message Bus
     ┌──────────────┼──────────────┐
     │              │              │
┌────┴─────┐  ┌─────┴────┐  ┌────┴─────┐
│결제 서비스│  │알림 서비스│  │배송 서비스│
│DB: Pay   │  │(Stateless)│  │DB: Ship  │
└──────────┘  └──────────┘  └──────────┘
```

| 비교 항목 | 모놀리식 | 마이크로서비스 |
|----------|---------|--------------|
| 개발 복잡도 | 초기 단순 | 높음 (분산 복잡성) |
| 배포 | 전체 재배포 | 서비스별 독립 배포 |
| 스케일링 | 전체 스케일 | 서비스별 독립 스케일 |
| 장애 격리 | 없음 (전체 장애) | 있음 (일부 장애) |
| 기술 스택 | 통일 | 서비스별 자유 선택 |
| 팀 규모 | 소~중규모 적합 | 중~대규모 적합 |
| 운영 복잡도 | 낮음 | 높음 (서비스 디스커버리, 분산 추적) |
| 데이터베이스 | 공유 DB | 서비스별 독립 DB |

> ⚠️ **주의**: 작은 팀에서 마이크로서비스는 오히려 독이 될 수 있다. Martin Fowler의 "Microservices Premium" — 관리 복잡도가 비즈니스 가치를 넘어서는 경우 모놀리식이 더 낫다.

### 6.2 12-Factor App 원칙

Heroku가 정의한 클라우드 네이티브 애플리케이션 설계 원칙 12가지다.

| # | 원칙 | 설명 | 실천 방법 |
|---|------|------|-----------|
| 1 | Codebase | 하나의 코드베이스, 다수 환경 배포 | Git 단일 저장소 |
| 2 | Dependencies | 의존성 명시적 선언·격리 | package.json, requirements.txt |
| 3 | Config | 설정을 환경 변수로 분리 | .env, K8s ConfigMap/Secret |
| 4 | Backing Services | 외부 서비스를 자원으로 취급 | DB, 캐시를 URL로 접근 |
| 5 | Build, Release, Run | 빌드·릴리스·실행 단계 분리 | CI/CD 파이프라인 |
| 6 | Processes | 무상태(stateless) 프로세스 | 세션을 Redis에 저장 |
| 7 | Port Binding | 포트 바인딩으로 서비스 제공 | HTTP 서버 내장 |
| 8 | Concurrency | 프로세스 모델로 스케일 아웃 | 수평 스케일링 |
| 9 | Disposability | 빠른 시작·정상 종료 | SIGTERM 처리, 빠른 부팅 |
| 10 | Dev/Prod Parity | 개발·스테이징·프로덕션 동일 | Docker로 환경 일치 |
| 11 | Logs | 로그를 이벤트 스트림으로 처리 | stdout/stderr 출력 |
| 12 | Admin Processes | 관리 작업을 일회성 프로세스로 | DB 마이그레이션 커맨드 |

### 6.3 불변 인프라 (Immutable Infrastructure)

**불변 인프라**는 한 번 배포된 서버를 절대 수정하지 않는 원칙이다. 변경이 필요하면 새 서버를 만들고 이전 서버를 폐기한다.

```
[가변 인프라 vs 불변 인프라]

가변 인프라 (Mutable)                불변 인프라 (Immutable)
─────────────────────────────        ─────────────────────────────
서버 v1                              서버 v1 (폐기)
  │                                    │
  ├── SSH 접속                         └── 삭제
  ├── apt upgrade                  
  ├── 설정 파일 변경                  서버 v2 (새로 생성)
  └── 서비스 재시작                     │
                                       ├── 이미지로 빌드
결과: "눈송이 서버"                    ├── 테스트
  - 예측 불가능                        └── 트래픽 전환
  - 재현 불가능
  - 드리프트 발생                    결과: 
                                      ✅ 완전한 재현성
                                      ✅ 드리프트 없음
                                      ✅ 롤백 용이
```

**컨테이너 이미지**가 불변 인프라의 가장 좋은 예다. `docker pull nginx:1.24.0`으로 당긴 이미지는 언제 어디서 실행해도 동일하다.

### 6.4 배포 전략

#### Blue/Green Deployment

```
[Blue/Green 배포]

현재 (Blue가 프로덕션)
                   ┌─────────────────┐
  사용자 트래픽 ──→│   Blue (v1.0)   │ ← 프로덕션
                   └─────────────────┘
                   ┌─────────────────┐
                   │  Green (v2.0)   │ ← 대기 (신버전 배포됨)
                   └─────────────────┘

전환 후 (Green이 프로덕션)
                   ┌─────────────────┐
                   │   Blue (v1.0)   │ ← 대기 (롤백 준비)
                   └─────────────────┘
                   ┌─────────────────┐
  사용자 트래픽 ──→│  Green (v2.0)  │ ← 프로덕션
                   └─────────────────┘
```

| 항목 | 내용 |
|------|------|
| 다운타임 | 거의 없음 (트래픽 전환 순간적) |
| 롤백 | 즉각적 (이전 환경 그대로 유지) |
| 비용 | 높음 (2배 인프라 필요) |
| 적합 환경 | 빠른 롤백이 중요한 경우 |

#### Canary Deployment

```
[Canary 배포]

1단계: 5% 카나리
  ┌─────────────────────────────────────┐
  │ 95% 트래픽 → v1.0 (기존)            │
  │  5% 트래픽 → v2.0 (카나리)          │
  └─────────────────────────────────────┘
             모니터링 (에러율, 응답시간)
             ↓ 문제없음
             
2단계: 25% 카나리
  ┌─────────────────────────────────────┐
  │ 75% 트래픽 → v1.0                   │
  │ 25% 트래픽 → v2.0                   │
  └─────────────────────────────────────┘
             ↓ 계속 문제없음
             
3단계: 100% (완전 전환)
  ┌─────────────────────────────────────┐
  │100% 트래픽 → v2.0                   │
  └─────────────────────────────────────┘
```

#### Rolling Update (롤링 업데이트)

```
[Rolling Update]

초기 상태: Pod 4개 모두 v1.0
[v1.0] [v1.0] [v1.0] [v1.0]

Step 1: 1개 교체
[v2.0] [v1.0] [v1.0] [v1.0]

Step 2: 1개 더 교체
[v2.0] [v2.0] [v1.0] [v1.0]

Step 3:
[v2.0] [v2.0] [v2.0] [v1.0]

완료:
[v2.0] [v2.0] [v2.0] [v2.0]
```

**배포 전략 비교**:

| 전략 | 다운타임 | 롤백 속도 | 비용 | 복잡도 |
|------|---------|----------|------|--------|
| Recreate | 있음 | 빠름 | 낮음 | 낮음 |
| Rolling Update | 없음 | 중간 | 낮음 | 낮음 |
| Blue/Green | 없음 | 즉각적 | 높음 (2배) | 중간 |
| Canary | 없음 | 빠름 | 중간 | 높음 |

### 6.5 Feature Flag (피처 플래그)

**Feature Flag**는 코드 배포 없이 기능을 켜고/끄는 메커니즘이다.

```python
# 간단한 피처 플래그 구현
import os

FEATURE_FLAGS = {
    "new_dashboard": os.getenv("FF_NEW_DASHBOARD", "false") == "true",
    "ai_recommendations": os.getenv("FF_AI_RECS", "false") == "true",
}

def get_dashboard(user_id):
    if FEATURE_FLAGS["new_dashboard"]:
        return render_new_dashboard(user_id)
    else:
        return render_old_dashboard(user_id)
```

**피처 플래그 활용 시나리오**:
- **A/B 테스팅**: 50% 사용자에게 새 UI 노출
- **점진적 롤아웃**: 내부 직원 → 베타 사용자 → 전체 사용자
- **킬 스위치**: 문제 발생 시 즉시 기능 비활성화
- **Trunk-Based Development**: 미완성 기능을 main에 병합하되 플래그로 숨김

**주요 피처 플래그 도구**: LaunchDarkly, Flagsmith, Unleash, GrowthBook

### 6.6 Chaos Engineering

**카오스 엔지니어링**은 프로덕션 시스템에 **의도적으로 장애를 주입**하여 시스템의 복원력(Resilience)을 검증하는 방식이다. Netflix의 **Chaos Monkey**가 원조다.

```
[Chaos Engineering 원칙]

1. 정상 상태(Steady State) 정의
   └── "초당 1000 요청, 에러율 0.1% 이하"

2. 가설 수립
   └── "서버 1대가 다운돼도 정상 상태를 유지할 것"

3. 장애 주입 (실험)
   └── 서버 종료, 네트워크 지연, CPU 과부하 등

4. 관찰 및 검증
   └── 정상 상태가 유지되는지 모니터링

5. 결과 학습 및 시스템 개선
   └── 취약점 발견 → 수정 → 재실험
```

**Chaos Engineering 도구**:

| 도구 | 대상 | 특징 |
|------|------|------|
| Chaos Monkey | AWS EC2 | Netflix 원조, VM 종료 |
| Chaos Toolkit | 멀티플랫폼 | 오픈소스, 확장 가능 |
| LitmusChaos | Kubernetes | K8s 네이티브, CNCF |
| Gremlin | 멀티플랫폼 | 상용, 풍부한 장애 유형 |
| AWS FIS | AWS | AWS Fault Injection Simulator |

> 💡 **핵심 요약**: 심화 개념들은 모두 **"미리 실패하는 것이 나중에 실패하는 것보다 낫다"**는 철학을 공유한다. 배포 전략은 위험을 최소화하고, 카오스 엔지니어링은 약점을 미리 발견하며, 불변 인프라는 환경 드리프트를 방지한다.

---

## 7. DevOps 메트릭과 KPI

### 7.1 DORA 메트릭

**DORA (DevOps Research and Assessment)** 는 Google이 지원하는 연구 조직으로, 6년 이상 수만 개 팀을 분석하여 DevOps 성과를 측정하는 4가지 핵심 메트릭을 정의했다.

```
[DORA 4가지 메트릭]

속도 (Velocity)                         안정성 (Stability)
┌──────────────────────┐               ┌──────────────────────┐
│ 배포 빈도             │               │ 변경 실패율           │
│ (Deployment          │               │ (Change Failure Rate)│
│  Frequency)          │               │                      │
│                      │               │ 배포 중 실패 비율     │
│ 얼마나 자주 배포하나?  │               │ = 실패 배포 / 전체    │
└──────────────────────┘               └──────────────────────┘
┌──────────────────────┐               ┌──────────────────────┐
│ 변경 리드 타임        │               │ 서비스 복구 시간      │
│ (Lead Time for       │               │ (MTTR)               │
│  Changes)            │               │                      │
│                      │               │ 장애 발생부터         │
│ 커밋 → 프로덕션 시간  │               │ 복구까지 시간         │
└──────────────────────┘               └──────────────────────┘
```

### 7.2 DORA 성능 등급

| 메트릭 | Elite (최상위) | High (상위) | Medium (중위) | Low (하위) |
|--------|--------------|-------------|--------------|-----------|
| **배포 빈도** | 하루 여러 번 | 하루 1번~주 1번 | 주 1번~월 1번 | 월 1번 미만 |
| **변경 리드 타임** | 1시간 미만 | 1일~1주 | 1주~6개월 | 6개월 초과 |
| **변경 실패율** | 5% 미만 | 10% 미만 | 15% 미만 | 45~60% |
| **MTTR** | 1시간 미만 | 1일 미만 | 1주 미만 | 6개월 초과 |

**2023 State of DevOps Report 기준 Elite 팀 특징**:
- 다른 팀 대비 **127배** 더 빠른 배포
- **8배** 더 짧은 변경 리드 타임
- **3배** 낮은 변경 실패율
- **6배** 빠른 장애 복구

### 7.3 SLA, SLO, SLI

```
[SLA / SLO / SLI 관계]

SLI (Service Level Indicator) — 측정 지표
  └── 실제로 측정되는 숫자
  └── 예: 지난 30일 가용성 = 99.95%

SLO (Service Level Objective) — 목표
  └── 팀이 달성하려는 목표
  └── 예: 30일 가용성 ≥ 99.9%

SLA (Service Level Agreement) — 계약
  └── 고객과의 법적 약속
  └── 예: 99.9% 가용성 미달 시 서비스 크레딧 지급

┌─────────────────────────────────────────────┐
│  SLA (고객과의 계약)                         │
│    └── SLO (내부 목표, SLA보다 엄격)         │
│          └── SLI (실제 측정값)              │
└─────────────────────────────────────────────┘

SLO를 SLA보다 엄격하게 설정 이유:
  SLI 99.95% > SLO 99.9% > SLA 99.5%
  SLO 달성 실패 → 경고
  SLA 달성 실패 → 고객 환불
```

**가용성 계산 — "나인즈"**:

| 가용성 | 연간 다운타임 | 표기 |
|--------|-------------|------|
| 90% | 36.5일 | "원 나인" |
| 99% | 3.65일 | "투 나인" |
| 99.9% | 8.76시간 | "쓰리 나인" |
| 99.99% | 52.6분 | "포 나인" |
| 99.999% | 5.26분 | "파이브 나인" |

**Error Budget (에러 버짓)**:

```
SLO = 99.9% (한 달 기준)
한 달 = 30일 × 24시간 × 60분 = 43,200분

Error Budget = 43,200분 × 0.1% = 43.2분

→ 이 달에 43분 이하 다운타임이면 OK
→ 43분 소진 시: 새 기능 배포 중단, 안정화 집중
→ 버짓이 남으면: 새 기능 실험 가능
```

### 7.4 추가 주요 메트릭

| 메트릭 | 의미 | 목표 |
|--------|------|------|
| MTTD (Mean Time to Detect) | 장애 감지까지 평균 시간 | 최소화 |
| MTTF (Mean Time to Failure) | 장애 발생 전 평균 운영 시간 | 최대화 |
| MTBF (Mean Time Between Failures) | 장애 간 평균 시간 | 최대화 |
| 배포 성공률 | 성공 배포 / 전체 배포 | 최대화 |
| 테스트 커버리지 | 테스트된 코드 비율 | 80%+ |
| 빌드 시간 | CI 파이프라인 완료 시간 | 10분 이내 |
| 컨테이너 재시작 횟수 | 비정상 재시작 | 최소화 |

> 💡 **핵심 요약**: DORA 메트릭은 DevOps 팀의 "건강 검진표"다. 4가지 지표 중 배포 빈도와 리드 타임은 속도, 변경 실패율과 MTTR은 안정성을 나타낸다. Elite 팀은 **빠르고 안정적인** 것이 양립 가능함을 증명했다.

---

## 8. DevOps 실무 시나리오

### 8.1 전체 DevOps 파이프라인 (코드 → 프로덕션)

```
[완전한 DevOps 파이프라인]

개발자 PC
┌─────────────────────────────────────────────────────────────────────┐
│  1. 코드 작성 (IDE + GitHub Copilot)                                 │
│  2. 로컬 테스트: docker compose up                                   │
│  3. git commit -m "feat: add payment feature"                       │
│  4. git push origin feature/payment                                 │
└──────────────────────────┬──────────────────────────────────────────┘
                           │
                    Pull Request 생성
                           │
                           ↓
GitHub / GitLab
┌─────────────────────────────────────────────────────────────────────┐
│  5. Pull Request 코드 리뷰                                           │
│  6. CI 자동 실행 (Trigger: PR 이벤트)                                │
│     ├── Lint & Static Analysis (ESLint, SonarQube)                  │
│     ├── Unit Tests (Jest) → Coverage Report                         │
│     ├── Security Scan (Trivy, Snyk)                                 │
│     └── Build Docker Image                                          │
│  7. PR 승인 → main 브랜치 머지                                       │
└──────────────────────────┬──────────────────────────────────────────┘
                           │ main push 이벤트
                           ↓
CI/CD 파이프라인 (GitHub Actions)
┌─────────────────────────────────────────────────────────────────────┐
│  8. 컨테이너 이미지 빌드                                             │
│     └── docker build -t myapp:$GIT_SHA .                            │
│  9. 이미지 푸시                                                      │
│     └── docker push ghcr.io/myorg/myapp:$GIT_SHA                   │
│  10. 스테이징 배포 (자동)                                            │
│     └── ArgoCD sync (staging 네임스페이스)                          │
│  11. 통합 테스트 / E2E 테스트 (스테이징에서 실행)                    │
│     └── Playwright, k6 성능 테스트                                  │
│  12. 프로덕션 배포 승인 요청 (Slack 알림)                            │
└──────────────────────────┬──────────────────────────────────────────┘
                           │ 수동 승인 (팀장 클릭)
                           ↓
프로덕션 배포
┌─────────────────────────────────────────────────────────────────────┐
│  13. Canary 배포 시작 (5% 트래픽)                                    │
│      └── ArgoCD + Argo Rollouts                                     │
│  14. 5분 모니터링 (에러율, 응답시간 기준 충족?)                       │
│      ├── 성공: 25% → 50% → 100% 트래픽 전환                         │
│      └── 실패: 자동 롤백                                            │
│  15. 배포 완료 알림 (Slack)                                          │
└─────────────────────────────────────────────────────────────────────┘
                           │
                           ↓
운영 모니터링
┌─────────────────────────────────────────────────────────────────────┐
│  16. Prometheus 메트릭 수집                                          │
│  17. Grafana 대시보드 확인                                           │
│  18. 알림: 에러율 > 1% → PagerDuty → 온콜 담당자                    │
└─────────────────────────────────────────────────────────────────────┘
```

### 8.2 장애 대응 플로우 (Incident Response)

```
[장애 대응 프로세스]

1. 감지 (Detection)
   Prometheus Alert → Alertmanager → PagerDuty
                                          │
                                          ↓
2. 인지 (Acknowledge)
   온콜 담당자 PagerDuty 확인 (목표: 5분 이내)
                 │
                 ↓
3. 분류 (Triage)
   ├── P1: 서비스 완전 중단 → 즉시 에스컬레이션
   ├── P2: 일부 기능 중단 → 1시간 내 해결
   └── P3: 성능 저하 → 4시간 내 해결
                 │
                 ↓
4. 초기 대응 (Mitigation)
   ├── 로그 확인: kubectl logs -f / ELK Kibana
   ├── 메트릭 확인: Grafana 대시보드
   ├── 최근 배포 확인: git log / ArgoCD 히스토리
   └── 롤백 판단:
       ├── 최근 배포가 원인? → 즉시 롤백
       └── 아니면? → 원인 파악 계속
                 │
                 ↓
5. 임시 조치 (Temporary Fix)
   ├── Pod 재시작: kubectl rollout restart deployment/myapp
   ├── 이전 버전 롤백: kubectl rollout undo deployment/myapp
   ├── 트래픽 우회: 로드 밸런서 설정 변경
   └── 기능 비활성화: Feature Flag OFF
                 │
                 ↓
6. 서비스 복구 확인
   모니터링 정상화 → 이해관계자 알림
                 │
                 ↓
7. 근본 원인 분석 (RCA)
   └── 72시간 내 Blameless Postmortem 작성
```

**장애 대응 필수 명령어**:

```bash
# 1. 빠른 상황 파악
kubectl get pods --all-namespaces | grep -v Running
kubectl get events --sort-by='.lastTimestamp' -n production

# 2. 문제 Pod 로그 확인
kubectl logs -f myapp-xxx --previous   # 이전 컨테이너 로그 (크래시 후)
kubectl logs myapp-xxx --tail=100      # 최근 100줄

# 3. Pod 상세 정보 (이벤트 포함)
kubectl describe pod myapp-xxx

# 4. 즉시 롤백
kubectl rollout undo deployment/myapp
kubectl rollout status deployment/myapp

# 5. 특정 버전으로 롤백
kubectl rollout history deployment/myapp
kubectl rollout undo deployment/myapp --to-revision=3

# 6. Pod 강제 재시작
kubectl rollout restart deployment/myapp

# 7. 리소스 사용량 확인
kubectl top pods -n production
kubectl top nodes
```

### 8.3 인프라 프로비저닝 워크플로우

```
[IaC 기반 인프라 프로비저닝 워크플로우]

요구사항
 │ "신규 마이크로서비스를 위한 EKS 클러스터 필요"
 ↓
1. 설계
   ├── 네트워크: VPC, 서브넷, 보안 그룹
   ├── 컴퓨팅: EKS 클러스터, 노드 그룹
   └── 스토리지: RDS, S3

 ↓
2. Terraform 코드 작성
   ├── modules/vpc/main.tf
   ├── modules/eks/main.tf
   └── environments/prod/main.tf

 ↓
3. 코드 리뷰 (PR)
   └── 팀원 검토 → 승인

 ↓
4. terraform plan (CI에서 자동 실행)
   └── 변경사항 미리 확인 (PR 코멘트로 표시)

 ↓
5. PR 머지 → terraform apply (자동 or 수동)
   └── 실제 AWS 리소스 생성

 ↓
6. Ansible로 초기 설정
   └── K8s 애드온 설치 (metrics-server, ingress-nginx)

 ↓
7. ArgoCD 설치 및 Git 저장소 연동
   └── GitOps 자동화 활성화

 ↓
8. 모니터링 스택 배포
   └── kube-prometheus-stack (Helm)

 ↓
완료: 애플리케이션 배포 준비 완료
```

---

## 9. DevOps 엔지니어 로드맵

### 9.1 필수 기술 스택

```
[DevOps 엔지니어 기술 맵]

기초 (Foundation)
├── Linux (이미 보유 ✅)
├── Networking (이미 보유 ✅)
├── 프로그래밍 (Python, Bash, Go)
└── Git (이미 보유 ✅)

컨테이너 & 오케스트레이션
├── Docker (필수)
├── Kubernetes (필수)
└── Helm (K8s 패키지 관리)

CI/CD
├── GitHub Actions (추천 시작점)
├── Jenkins (선택)
└── ArgoCD (GitOps)

인프라 관리
├── Terraform (필수)
├── Ansible (추천)
└── Vault (시크릿 관리)

클라우드
├── AWS (추천 시작점, 점유율 1위)
├── Azure (선택)
└── GCP (선택)

모니터링 & 로깅
├── Prometheus + Grafana
├── ELK Stack 또는 Loki
└── OpenTelemetry

보안
├── Trivy (컨테이너 스캔)
├── SOPS / Vault (시크릿 관리)
└── OPA / Kyverno (정책)
```

### 9.2 학습 순서 추천 (광주소마고 기준)

광주소프트웨어마이스터고 입학 전/후 학습 로드맵 (리눅스 마스터 2급, 네트워크 관리사 2급 보유 기준):

```
Phase 1 — 기초 강화 (1~2개월)
────────────────────────────────────
[ ] 리눅스 심화
    - 파일시스템, 프로세스, 네트워크 설정
    - 쉘 스크립팅 (Bash)
    - systemd, cron
[ ] Git / GitHub 실습
    - Git Flow 브랜치 전략
    - GitHub Actions 기초
[ ] Python 기초 (자동화 스크립팅)
    - 파일 처리, requests, boto3

Phase 2 — 컨테이너 (2~3개월)
────────────────────────────────────
[ ] Docker 기초 → 심화
    - Dockerfile 작성
    - Docker Compose
    - 멀티스테이지 빌드
[ ] Docker로 미니 프로젝트
    - 간단한 웹앱 컨테이너화

Phase 3 — CI/CD 파이프라인 (2개월)
────────────────────────────────────
[ ] GitHub Actions
    - 기본 워크플로우
    - 시크릿 관리
    - Matrix Build
[ ] Jenkins (선택)
    - Declarative Pipeline
[ ] 프로젝트: Node.js/Python 앱의 CI/CD

Phase 4 — Kubernetes (3개월)
────────────────────────────────────
[ ] K8s 아키텍처 이해
[ ] minikube / k3s 로컬 실습
[ ] Deployment, Service, Ingress
[ ] ConfigMap, Secret
[ ] RBAC
[ ] Helm 차트 작성
[ ] 프로젝트: K8s에 전체 스택 배포

Phase 5 — IaC & 클라우드 (3개월)
────────────────────────────────────
[ ] AWS 핵심 서비스
    - EC2, VPC, S3, RDS, IAM
    - EKS, ECR
[ ] Terraform 기초 → 심화
    - Module 작성
    - Remote State
    - Workspace
[ ] 프로젝트: Terraform으로 AWS 인프라 구축

Phase 6 — 모니터링 & 심화 (2개월)
────────────────────────────────────
[ ] Prometheus + Grafana
[ ] ELK Stack 또는 Loki
[ ] ArgoCD (GitOps)
[ ] 보안: Trivy, Vault

Phase 7 — 포트폴리오 프로젝트 (상시)
────────────────────────────────────
[ ] 전체 스택 통합 프로젝트
    (GitHub → CI/CD → Docker → K8s → 모니터링)
```

### 9.3 자격증 및 인증

| 자격증 | 발급 기관 | 수준 | 추천 시점 | 비용 |
|--------|----------|------|-----------|------|
| **AWS SAA** (Solutions Architect Associate) | AWS | 중급 | Phase 5 완료 후 | $150 |
| **CKA** (Certified Kubernetes Administrator) | CNCF | 중급 | Phase 4 완료 후 | $395 |
| **CKAD** (Certified K8s App Developer) | CNCF | 중급 | CKA 이후 | $395 |
| **Terraform Associate** | HashiCorp | 초~중급 | Phase 5 중 | $70 |
| **AWS DevOps Professional** | AWS | 고급 | SAA 이후 | $300 |
| **CKS** (Certified K8s Security Specialist) | CNCF | 고급 | CKA 이후 | $395 |
| **GCP Associate Cloud Engineer** | Google | 중급 | GCP 학습 후 | $200 |

**자격증 취득 우선순위 추천**:
1. **Terraform Associate** (상대적으로 저렴, 빠른 취득)
2. **AWS SAA** (가장 넓은 인지도)
3. **CKA** (K8s 실력 증명)

**시험 팁**:
- CKA/CKAD는 온라인 실기 시험 (kubectl 직접 입력)
- Killer.sh 모의시험 2회 제공 (실제보다 어려움)
- AWS SAA는 화이트페이퍼 + 덤프 + AWS 공식 연습 문제

### 9.4 포트폴리오 구성 팁

**GitHub 포트폴리오 핵심 프로젝트**:

```
추천 포트폴리오 프로젝트 (난이도 순)

1. [기초] 간단한 웹앱 CI/CD
   ─────────────────────────
   - Node.js/Python Flask 앱
   - GitHub Actions (lint + test + build)
   - Docker Hub에 이미지 푸시
   - 기술: Git, Docker, GitHub Actions

2. [중급] Kubernetes 배포 자동화
   ─────────────────────────────
   - 위 앱을 K8s에 배포
   - Helm 차트 작성
   - ArgoCD GitOps 설정
   - Ingress + TLS (Let's Encrypt)
   - 기술: K8s, Helm, ArgoCD, cert-manager

3. [중급] IaC로 클라우드 인프라 구축
   ──────────────────────────────────
   - Terraform으로 AWS VPC + EKS 구성
   - Ansible로 초기 설정 자동화
   - Remote State (S3 + DynamoDB Lock)
   - 기술: Terraform, Ansible, AWS

4. [고급] 완전한 DevOps 파이프라인
   ─────────────────────────────────
   - 마이크로서비스 2~3개
   - 전체 CI/CD 파이프라인
   - 모니터링 (Prometheus + Grafana)
   - 로깅 (Loki)
   - Canary 배포 (Argo Rollouts)
   - 보안 스캔 (Trivy, Snyk)
   - 기술: 모든 것의 종합

5. [심화] Chaos Engineering 실험
   ─────────────────────────────
   - LitmusChaos 설치
   - 장애 시나리오 정의 및 실험
   - 결과 리포트 작성
```

**포트폴리오 작성 팁**:
- **README.md 중요**: 아키텍처 다이어그램 포함, 실행 방법 명확히
- **실제 동작하는 데모 링크** 포함 (AWS Free Tier 활용)
- **문제 해결 과정** 블로그/노션에 기록 (트러블슈팅 경험)
- **코드 품질**: 시크릿을 절대 커밋하지 않음, .gitignore 관리
- GitHub Profile README에 스킬 뱃지 및 프로젝트 요약

**학습 자원 추천**:

| 자원 | 유형 | URL |
|------|------|-----|
| Roadmap.sh | 로드맵 시각화 | https://roadmap.sh/devops |
| KillerCoda | K8s 브라우저 실습 | https://killercoda.com |
| Play with K8s | K8s 무료 실습 환경 | https://labs.play-with-k8s.com |
| AWS Free Tier | AWS 무료 실습 | https://aws.amazon.com/free |
| TechWorld with Nana | YouTube | 한국어 자막 있음 |
| DevOps Toolkit | YouTube | 실무 중심 |

---

## 10. 참고 자료 및 링크

### 공식 문서

| 리소스 | URL |
|--------|-----|
| Kubernetes 공식 문서 | https://kubernetes.io/docs |
| Docker 공식 문서 | https://docs.docker.com |
| Terraform 공식 문서 | https://developer.hashicorp.com/terraform |
| Ansible 공식 문서 | https://docs.ansible.com |
| GitHub Actions 문서 | https://docs.github.com/en/actions |
| Prometheus 문서 | https://prometheus.io/docs |
| ArgoCD 문서 | https://argo-cd.readthedocs.io |

### 필독 도서

| 책 | 저자 | 핵심 내용 |
|----|------|----------|
| **The Phoenix Project** | Gene Kim 외 | DevOps 소설, 문화 이해 |
| **The DevOps Handbook** | Gene Kim, Jez Humble 외 | DevOps 실천 가이드 |
| **Site Reliability Engineering** | Google SRE Team | SRE 원론 (무료 온라인) |
| **Kubernetes in Action** | Marko Lukša | K8s 심화 |
| **Infrastructure as Code** | Kief Morris | IaC 원칙과 실천 |
| **Accelerate** | Nicole Forsgren 외 | DORA 연구 기반, 데이터로 증명 |

> 📌 "The Phoenix Project"는 소설 형식이라 읽기 쉽다. DevOps 입문자에게 가장 먼저 추천한다.

### 커뮤니티 & 뉴스

| 자원 | URL | 특징 |
|------|-----|------|
| CNCF (Cloud Native Computing Foundation) | https://cncf.io | K8s, Prometheus 등 관리 |
| DevOps.com | https://devops.com | 뉴스, 트렌드 |
| The New Stack | https://thenewstack.io | 클라우드 네이티브 심층 기사 |
| Reddit r/devops | https://reddit.com/r/devops | 커뮤니티 Q&A |
| DORA State of DevOps Report | https://dora.dev | 연간 DevOps 현황 보고서 |

### 실습 환경

| 환경 | URL | 특징 |
|------|-----|------|
| KillerCoda | https://killercoda.com | 브라우저 K8s 실습 |
| Play with Docker | https://labs.play-with-docker.com | 무료 Docker 환경 |
| AWS Free Tier | https://aws.amazon.com/free | 12개월 무료 |
| GitHub Codespaces | https://github.com/features/codespaces | 클라우드 개발 환경 |
| Minikube | https://minikube.sigs.k8s.io | 로컬 K8s |
| K3d | https://k3d.io | Docker 기반 경량 K8s |

---

## 부록: 주요 약어 정리

| 약어 | 풀이 | 의미 |
|------|------|------|
| CI | Continuous Integration | 지속적 통합 |
| CD | Continuous Delivery/Deployment | 지속적 전달/배포 |
| IaC | Infrastructure as Code | 인프라를 코드로 |
| SRE | Site Reliability Engineering | 사이트 신뢰성 엔지니어링 |
| IDP | Internal Developer Platform | 내부 개발자 플랫폼 |
| DORA | DevOps Research and Assessment | DevOps 연구 및 평가 |
| MTTR | Mean Time To Recovery | 평균 복구 시간 |
| SLA | Service Level Agreement | 서비스 수준 계약 |
| SLO | Service Level Objective | 서비스 수준 목표 |
| SLI | Service Level Indicator | 서비스 수준 지표 |
| CALMS | Culture, Automation, Lean, Measurement, Sharing | DevOps 핵심 가치 |
| K8s | Kubernetes | 쿠버네티스 (컨테이너 오케스트레이션) |
| HCL | HashiCorp Configuration Language | Terraform 언어 |
| VSM | Value Stream Mapping | 가치 흐름 매핑 |
| RBAC | Role-Based Access Control | 역할 기반 접근 제어 |
| OPA | Open Policy Agent | 오픈 정책 에이전트 |
| CKA | Certified Kubernetes Administrator | K8s 관리자 자격증 |
| SAA | Solutions Architect Associate | AWS 솔루션 아키텍트 |
| SAST | Static Application Security Testing | 정적 애플리케이션 보안 테스트 |
| DAST | Dynamic Application Security Testing | 동적 애플리케이션 보안 테스트 |
| SCA | Software Composition Analysis | 소프트웨어 구성 분석 |
| APM | Application Performance Monitoring | 애플리케이션 성능 모니터링 |
| CNCF | Cloud Native Computing Foundation | 클라우드 네이티브 컴퓨팅 재단 |

---

## 한눈에 보는 DevOps 전체 그림

```
┌─────────────────────────────────────────────────────────────────────┐
│                        DevOps 전체 그림                              │
│                                                                     │
│  문화 (Culture)                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  협업 · 투명성 · 공동책임 · 심리적 안전감 · 지속적 학습    │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              ↕                                      │
│  라이프사이클 (∞ 무한루프)                                          │
│  Plan→Code→Build→Test→Release→Deploy→Operate→Monitor               │
│                              ↕                                      │
│  실천방법 (Practices)                                               │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐             │
│  │ CI/CD    │ │ IaC      │ │ 컨테이너 │ │ GitOps   │             │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘             │
│                              ↕                                      │
│  도구 (Tools)                                                       │
│  Git · Docker · K8s · Terraform · Ansible · Prometheus · ELK       │
│                              ↕                                      │
│  측정 (Measurement)                                                 │
│  DORA 메트릭 · SLA/SLO/SLI · Error Budget                         │
│                                                                     │
│  목표: 빠르고 안정적으로 고객에게 가치 전달                          │
└─────────────────────────────────────────────────────────────────────┘
```

---

*최종 업데이트: 2025년*  
*문서 분류: DevOps 중급 학습 자료*  
*참고: 이 문서는 지속적으로 업데이트됩니다. DevOps 생태계는 빠르게 변하므로 공식 문서를 항상 함께 참고하세요.*
