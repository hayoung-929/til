# 🐳 Docker 완전 정복 TIL

---

## 1️⃣ 도커 기본 개념

### 📌 도커란? (등장 배경, 가상화의 역사)

#### 가상화의 역사
```
1960s  → IBM 메인프레임에서 시작된 하드웨어 분리 개념
1990s  → VMware 등장, Type-1 / Type-2 하이퍼바이저
2000s  → KVM, Xen 등 오픈소스 하이퍼바이저 확산
2008   → LXC(Linux Containers) 등장 - 커널 레벨 격리
2013   → Docker 등장 (Solomon Hykes, dotCloud)
2014~  → 컨테이너 오케스트레이션 시대 (Kubernetes 등장)
```

#### 도커 등장 배경
- **"내 PC에서는 됩니다" 문제 해결**: 개발/운영 환경 불일치
- 배포 시 라이브러리 충돌, OS 의존성 문제 반복
- VM은 너무 무겁고 느림 → 더 가벼운 격리 단위 필요
- 클라우드 네이티브 시대 → 빠른 배포/확장 수요 폭증

#### 도커가 해결한 것
| 문제 | 기존 | Docker |
|------|------|--------|
| 환경 차이 | "내 PC에선 되는데..." | 이미지로 환경 통일 |
| 배포 속도 | VM 부팅 수 분 | 컨테이너 수 초 |
| 자원 효율 | OS 전체 올림 | 커널 공유 |
| 확장성 | VM 복제 느림 | 이미지 레이어 캐시 |

---

### 📌 VM vs 컨테이너 차이

![VM vs 컨테이너 구조](./img/docker_vm_container.svg)

| 구분 | VM | 컨테이너 |
|------|----|----|
| 격리 단위 | 하드웨어 수준 | 프로세스 수준 |
| 부팅 시간 | 수 분 | 수 초 (밀리초) |
| 이미지 크기 | GB 단위 | MB 단위 |
| OS | 독립적 Guest OS | Host OS 커널 공유 |
| 자원 오버헤드 | 높음 | 낮음 |
| 보안 격리 | 강함 | 상대적으로 약함 |
| 사용 사례 | 다른 OS 필요, 강한 격리 | 마이크로서비스, CI/CD |

#### 핵심 격리 기술
- **Namespace**: 프로세스(PID), 네트워크, 마운트, UTS, IPC, User 격리
- **cgroups**: CPU, 메모리, I/O 리소스 제한
- **Union File System**: 레이어 기반 파일 시스템 (OverlayFS)

---

### 📌 도커 아키텍처 (Engine, Daemon, CLI)

![도커 아키텍처](./img/docker_architecture.svg)

| 구성 요소 | 역할 |
|-----------|------|
| Docker CLI | 사용자 명령 입력, REST API로 daemon에 전달 |
| dockerd | 메인 데몬, 이미지/컨테이너/네트워크/볼륨 관리 |
| containerd | 컨테이너 런타임 관리 (CNCF 프로젝트) |
| containerd-shim | 컨테이너 프로세스와 daemon 분리 (daemon 재시작 시 컨테이너 유지) |
| runc | OCI 표준 컨테이너 런타임, 실제 컨테이너 생성/실행 |

---

### 📌 도커의 장점 & 활용 사례

#### 장점
- **환경 일관성**: Dev = Staging = Prod
- **빠른 배포**: 이미지 pull 후 즉시 실행
- **격리성**: 앱 간 의존성 충돌 없음
- **이식성**: 어디서든 동일하게 동작
- **버전 관리**: 이미지 태그로 롤백 가능
- **경량성**: VM 대비 자원 효율적

#### 주요 활용 사례
```
1. 로컬 개발 환경 통일      → docker-compose로 DB/캐시 등 통합
2. CI/CD 파이프라인         → 빌드/테스트/배포 컨테이너화
3. 마이크로서비스 배포       → 각 서비스 독립 컨테이너
4. 레거시 앱 마이그레이션    → 의존성 그대로 패키징
5. 임시 테스트 환경          → 빠르게 올리고 삭제
6. 서버리스 컨테이너         → AWS Fargate, Cloud Run
```

---

### 📌 설치 및 환경 설정

#### Windows
```powershell
# Docker Desktop 설치 (WSL2 백엔드 권장)
# 1. WSL2 활성화
wsl --install
wsl --set-default-version 2

# 2. Docker Desktop 다운로드 후 설치
# https://www.docker.com/products/docker-desktop

# 3. 설치 확인
docker version
docker run hello-world
```

#### macOS
```bash
# Homebrew 방법
brew install --cask docker

# 또는 Docker Desktop dmg 직접 설치
# Apple Silicon(M1/M2)은 ARM 버전 설치
docker version
docker run hello-world
```

#### Linux (Ubuntu)
```bash
# 1. 기존 버전 제거
sudo apt remove docker docker-engine docker.io containerd runc

# 2. 의존성 설치
sudo apt update
sudo apt install ca-certificates curl gnupg

# 3. GPG 키 추가
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# 4. 저장소 추가
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 5. Docker Engine 설치
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 6. 현재 유저를 docker 그룹에 추가 (sudo 없이 사용)
sudo usermod -aG docker $USER
newgrp docker

# 7. 확인
docker version
```

---

### 📌 Docker CE vs EE

| 구분 | CE (Community Edition) | EE (Enterprise Edition) |
|------|----------------------|------------------------|
| 대상 | 개발자, 소규모 팀 | 기업, 대규모 운영 |
| 비용 | 무료 | 유료 |
| 지원 | 커뮤니티 | 공식 기술 지원 |
| 기능 | 기본 도커 기능 | RBAC, 이미지 서명, UCP |
| 현황 | 현재 주류 | Docker Swarm EE → Mirantis 인수 |

> 💡 현재는 대부분 CE(또는 Docker Engine) + Kubernetes 조합 사용

---

### 📌 OCI 표준 (containerd, runc 관계)

```
OCI (Open Container Initiative) - 2015년 설립
├── Image Spec    : 이미지 포맷 표준
├── Runtime Spec  : 컨테이너 실행 표준
└── Distribution Spec : 이미지 배포 표준

계층 구조:
Docker CLI
   └── dockerd (Docker Daemon)
         └── containerd  [CNCF 프로젝트, 런타임 관리]
               └── runc  [OCI Runtime Spec 구현체, 실제 실행]
```

- **OCI**: 컨테이너 표준화 단체 → 특정 벤더 종속 방지
- **containerd**: 이미지 pull/push, 스냅샷, 컨테이너 생명주기 관리
- **runc**: Linux namespace, cgroups 직접 조작하여 컨테이너 프로세스 생성

---

## 2️⃣ 이미지 (Image)

### 📌 이미지의 개념

- 컨테이너 실행에 필요한 **파일 시스템 + 메타데이터**를 담은 읽기 전용 템플릿
- 컨테이너는 이미지 위에 **쓰기 레이어(Writable Layer)** 를 추가한 것
- 이미지 자체는 불변(Immutable) → 실행마다 동일한 환경 보장

```
이미지 = 설계도 (Read-Only)
컨테이너 = 실제 실행 인스턴스 (이미지 + Writable Layer)

같은 이미지로 N개의 컨테이너 생성 가능 ✅
```

#### 이미지 식별자
```
[레지스트리]/[이름]:[태그]

docker.io/library/nginx:1.25      # Docker Hub 공식 이미지
myregistry.io/myapp:v2.1.0       # Private Registry
ghcr.io/user/repo:latest          # GitHub Container Registry
```

---

### 📌 Layer 구조와 OverlayFS 원리

#### 레이어 구조
```
컨테이너 실행 시:
┌─────────────────────────────┐
│  Writable Layer (Container) │  ← 컨테이너마다 독립
├─────────────────────────────┤
│  Layer 4: COPY app /app     │  ← Read-Only
├─────────────────────────────┤
│  Layer 3: RUN pip install   │  ← Read-Only (캐시됨)
├─────────────────────────────┤
│  Layer 2: RUN apt update    │  ← Read-Only (캐시됨)
├─────────────────────────────┤
│  Layer 1: FROM python:3.11  │  ← Read-Only (캐시됨)
└─────────────────────────────┘
```

#### OverlayFS 원리
```
OverlayFS 구성:
- lowerdir: 이미지 레이어들 (Read-Only)
- upperdir: 컨테이너 쓰기 레이어 (Read-Write)
- workdir:  임시 작업 디렉토리
- merged:   사용자에게 보이는 통합 뷰

파일 수정 시 → Copy-on-Write (CoW):
  파일을 upperdir로 복사 후 수정 → lowerdir는 변경 없음

실제 경로:
/var/lib/docker/overlay2/[hash]/
  ├── diff/     (각 레이어 실제 파일)
  ├── link      (레이어 ID 심볼릭 링크)
  └── merged/   (통합 마운트 포인트)
```

#### 레이어 캐시의 중요성
```dockerfile
# ❌ 나쁜 예 - 코드 변경 시 pip install까지 재실행
FROM python:3.11
COPY . /app          # 소스 변경 → 이후 모든 레이어 캐시 무효화
RUN pip install -r requirements.txt

# ✅ 좋은 예 - 의존성 먼저, 소스 나중
FROM python:3.11
COPY requirements.txt /app/   # 의존성만 먼저 → 캐시 재사용
RUN pip install -r /app/requirements.txt
COPY . /app                   # 소스 변경은 여기서만 영향
```

---

### 📌 Dockerfile 기본 문법

```dockerfile
# ==========================================
# 기본 구조 예시
# ==========================================

# FROM: 베이스 이미지 지정 (필수, 첫 번째 명령)
FROM node:20-alpine

# LABEL: 이미지 메타데이터
LABEL maintainer="yourname@email.com"
LABEL version="1.0"

# ARG: 빌드 시 변수 (빌드 중에만 유효)
ARG NODE_ENV=production
ARG APP_PORT=3000

# ENV: 환경변수 (런타임에도 유지)
ENV NODE_ENV=${NODE_ENV}
ENV PORT=${APP_PORT}

# WORKDIR: 작업 디렉토리 설정 (없으면 자동 생성)
WORKDIR /app

# COPY: 파일/디렉토리 복사 (권장)
COPY package*.json ./

# RUN: 빌드 중 명령 실행 (레이어 생성)
RUN npm ci --only=production

# COPY 나머지 소스
COPY . .

# EXPOSE: 컨테이너가 사용할 포트 문서화 (실제 포트 열지 않음)
EXPOSE 3000

# VOLUME: 볼륨 마운트 포인트 선언
VOLUME ["/app/data"]

# USER: 실행 유저 변경 (보안)
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# HEALTHCHECK: 컨테이너 상태 확인
HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD wget -q --spider http://localhost:3000/health || exit 1

# CMD: 기본 실행 명령 (덮어쓰기 가능)
CMD ["node", "server.js"]
```

#### 각 명령어 상세

| 명령어 | 설명 | 레이어 생성 |
|--------|------|------------|
| `FROM` | 베이스 이미지 | ✅ |
| `RUN` | 빌드 중 셸 명령 실행 | ✅ |
| `COPY` | 호스트 → 이미지 파일 복사 | ✅ |
| `ADD` | COPY + URL/tar 지원 | ✅ |
| `CMD` | 기본 실행 명령 (덮어쓰기 가능) | ❌ |
| `ENTRYPOINT` | 고정 실행 명령 | ❌ |
| `ENV` | 환경변수 | ✅ |
| `ARG` | 빌드 인자 | ❌ |
| `EXPOSE` | 포트 문서화 | ❌ |
| `WORKDIR` | 작업 디렉토리 | ✅ |
| `USER` | 실행 유저 | ❌ |
| `VOLUME` | 볼륨 선언 | ✅ |
| `LABEL` | 메타데이터 | ✅ |
| `HEALTHCHECK` | 헬스체크 설정 | ❌ |
| `ONBUILD` | 자식 이미지 빌드 시 실행 | ❌ |

---

### 📌 CMD vs ENTRYPOINT

```
핵심 차이:
ENTRYPOINT → 반드시 실행되는 명령 (컨테이너의 "정체성")
CMD        → 기본 인자 / 기본 명령 (덮어쓰기 가능)
```

#### 형식

```dockerfile
# Shell 형식 - /bin/sh -c 로 실행 (신호 전달 문제 있음)
CMD node server.js
ENTRYPOINT node server.js

# Exec 형식 - 직접 실행 (권장, PID 1이 됨)
CMD ["node", "server.js"]
ENTRYPOINT ["node", "server.js"]
```

#### 조합 예시

```dockerfile
# Case 1: CMD만
CMD ["nginx", "-g", "daemon off;"]
# docker run myimage          → nginx -g daemon off; 실행
# docker run myimage bash     → bash 실행 (CMD 완전 교체)

# Case 2: ENTRYPOINT만
ENTRYPOINT ["nginx"]
# docker run myimage          → nginx 실행
# docker run myimage -t       → nginx -t 실행 (인자 추가)

# Case 3: ENTRYPOINT + CMD (권장 패턴)
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
# docker run myimage              → nginx -g daemon off;
# docker run myimage -t           → nginx -t  (CMD 교체)
# docker run --entrypoint bash myimage  → bash (ENTRYPOINT 교체)
```

#### 실전 패턴

```dockerfile
# 스크립트 래퍼 패턴 (초기화 로직 실행 후 앱 시작)
COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["node", "server.js"]

# docker-entrypoint.sh
#!/bin/sh
# 초기화 작업 (마이그레이션, 환경 설정 등)
echo "Starting initialization..."
exec "$@"  # CMD 인자를 PID 1로 실행
```

---

### 📌 COPY vs ADD

```dockerfile
# COPY: 단순 복사 (권장)
COPY src/ /app/src/
COPY package.json package-lock.json ./

# ADD: 추가 기능 (남용 금지)
ADD https://example.com/file.tar.gz /tmp/   # URL 다운로드 가능
ADD archive.tar.gz /app/                     # tar 자동 압축 해제
```

| 기능 | COPY | ADD |
|------|------|-----|
| 파일/디렉토리 복사 | ✅ | ✅ |
| URL 다운로드 | ❌ | ✅ |
| tar 자동 해제 | ❌ | ✅ |
| 권장 여부 | ✅ 권장 | ⚠️ 필요시만 |

> ✅ **원칙**: URL 다운로드는 `RUN curl`로, tar 해제도 `RUN tar`로 명시적으로 하는 게 더 투명함. COPY를 기본으로 사용하고 ADD는 tar 자동 해제 용도로만 제한적 사용.

---

### 📌 이미지 빌드 & 멀티스테이지 빌드

#### 기본 빌드
```bash
# 기본 빌드
docker build -t myapp:1.0 .

# 빌드 컨텍스트 지정
docker build -t myapp:1.0 -f Dockerfile.prod ./src

# 빌드 인자 전달
docker build --build-arg NODE_ENV=production -t myapp:1.0 .

# 캐시 무시
docker build --no-cache -t myapp:1.0 .

# 특정 스테이지까지만 빌드
docker build --target builder -t myapp-builder .
```

#### 멀티스테이지 빌드

```dockerfile
# ========================================
# Stage 1: 빌드 환경 (크고 무거운 이미지)
# ========================================
FROM node:20 AS builder

WORKDIR /build
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build      # dist/ 생성

# ========================================
# Stage 2: 실행 환경 (가볍게!)
# ========================================
FROM node:20-alpine AS runner

WORKDIR /app
ENV NODE_ENV=production

COPY package*.json ./
RUN npm ci --only=production

# 빌드 결과물만 복사
COPY --from=builder /build/dist ./dist

USER node
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

```bash
# 최종 이미지에는 빌드 도구 없음!
# Before: 800MB → After: 150MB
```

#### Go 언어 멀티스테이지 (극단적 경량화)
```dockerfile
FROM golang:1.21 AS builder
WORKDIR /src
COPY go.* ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /app ./cmd/server

# Scratch: 아무것도 없는 빈 이미지
FROM scratch
COPY --from=builder /app /app
ENTRYPOINT ["/app"]
# 최종 이미지: ~10MB
```

---

### 📌 .dockerignore

```
# .dockerignore 예시

# Node.js
node_modules/
npm-debug.log*

# 빌드 산출물
dist/
build/
.cache/

# 버전 관리
.git/
.gitignore

# 환경설정 (보안!)
.env
.env.local
*.env

# 에디터/OS
.DS_Store
.vscode/
*.swp
Thumbs.db

# 테스트
coverage/
*.test.js
__tests__/

# Docker 관련 (빌드 컨텍스트 불필요)
Dockerfile*
docker-compose*.yml
```

> 🎯 **.dockerignore가 중요한 이유**
> - 빌드 컨텍스트 크기 감소 → 빌드 속도 향상
> - node_modules 등 불필요한 파일 이미지 포함 방지
> - 민감 정보(.env) 이미지에 포함되는 사고 방지

---

### 📌 이미지 Push / Pull (Docker Hub)

```bash
# 로그인
docker login
docker login -u username -p password   # 비대화형

# 태그 설정
docker tag myapp:1.0 username/myapp:1.0
docker tag myapp:1.0 username/myapp:latest

# Push
docker push username/myapp:1.0
docker push username/myapp:latest

# Pull
docker pull username/myapp:1.0
docker pull nginx                      # = nginx:latest

# 이미지 목록 확인
docker images
docker images | grep myapp

# 이미지 삭제
docker rmi username/myapp:1.0
docker image prune         # dangling 이미지 삭제
docker image prune -a      # 사용 안 하는 모든 이미지 삭제
```

---

### 📌 태그 전략

```bash
# Semantic Versioning
myapp:2.1.3         # 정확한 버전 (프로덕션 권장)
myapp:2.1           # 마이너 버전
myapp:2             # 메이저 버전
myapp:latest        # 최신 (개발/테스트용, 프로덕션 지양)

# 환경별 태그
myapp:dev           # 개발용
myapp:staging       # 스테이징용
myapp:prod-2024010  # 프로덕션 + 날짜

# Git 커밋 해시
myapp:a3f9b2c       # 특정 커밋 추적 가능

# CI/CD 태그 예시 (GitHub Actions)
myapp:main-$(git rev-parse --short HEAD)
myapp:pr-123
```

> ⚠️ **latest 태그 주의사항**
> - `latest`는 자동으로 최신이 아님 → 명시적으로 태그해야 함
> - 프로덕션에서 `latest` 사용 시 예기치 않은 버전 변경 위험
> - 항상 구체적인 버전 태그로 고정 권장

---

### 📌 이미지 취약점 검사 (docker scout)

```bash
# Docker Scout 사용 (Docker Desktop 내장)
docker scout quickview myapp:1.0         # 빠른 취약점 개요
docker scout cves myapp:1.0             # CVE 목록 상세
docker scout recommendations myapp:1.0  # 개선 권고사항

# 심각도 필터
docker scout cves --only-severity critical,high myapp:1.0

# CI에서 취약점 발견 시 실패
docker scout cves --exit-code myapp:1.0

# Trivy (오픈소스 대안)
trivy image myapp:1.0
trivy image --severity HIGH,CRITICAL myapp:1.0
```

---

### 📌 베이스 이미지 선택

| 이미지 | 크기 | 특징 | 사용 사례 |
|--------|------|------|----------|
| `ubuntu:22.04` | ~80MB | 풀 기능 OS | 디버깅, 레거시 |
| `debian:bookworm-slim` | ~75MB | Debian 경량 | 일반 서비스 |
| `alpine:3.19` | ~7MB | musl libc, BusyBox | 경량화 최우선 |
| `node:20-alpine` | ~60MB | Alpine 기반 Node | Node 앱 |
| `node:20-slim` | ~200MB | Debian slim 기반 | 호환성 필요 시 |
| `gcr.io/distroless/nodejs20` | ~110MB | 쉘 없는 최소 이미지 | 보안 최우선 |
| `scratch` | 0MB | 완전 비어있음 | Go 등 정적 바이너리 |

#### 선택 기준
```
보안 최우선        → distroless
크기 최우선        → alpine (단, musl 호환성 확인)
호환성 최우선      → debian-slim
빠른 개발         → 공식 이미지 (ubuntu, node 등)
정적 바이너리      → scratch
```

#### Alpine 주의사항
```dockerfile
# Alpine은 musl libc 사용 → glibc 의존 앱 호환 문제 있을 수 있음
# 패키지 매니저: apk (apt 아님)
RUN apk add --no-cache curl wget bash

# 타임존 설정
RUN apk add --no-cache tzdata
ENV TZ=Asia/Seoul
```
## 3️⃣ 컨테이너 (Container)

### 📌 컨테이너 개념 및 생명주기

```
컨테이너 = 이미지를 실행한 인스턴스
         = 격리된 프로세스 (Namespace + cgroups)
```

#### 생명주기 (Lifecycle)

![컨테이너 생명주기](./img/docker_lifecycle.svg)

---

### 📌 주요 명령어

```bash
# ────────── 생성 & 실행 ──────────
docker run nginx                          # 실행 (없으면 pull)
docker run -d nginx                       # 백그라운드 실행
docker run -it ubuntu bash                # 대화형 실행
docker run --name mycontainer nginx       # 이름 지정
docker run --rm nginx                     # 종료 시 자동 삭제

docker create --name myc nginx            # 생성만 (미실행)
docker start mycontainer                  # 시작
docker stop mycontainer                   # 정상 종료 (SIGTERM)
docker kill mycontainer                   # 강제 종료 (SIGKILL)
docker restart mycontainer                # 재시작

# ────────── 목록 & 상태 ──────────
docker ps                                 # 실행 중인 컨테이너
docker ps -a                              # 모든 컨테이너 (종료 포함)
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"

# ────────── 접속 & 명령 실행 ──────────
docker exec -it mycontainer bash          # 실행 중 컨테이너 접속
docker exec mycontainer ls /app           # 단순 명령 실행
docker exec -u root mycontainer bash      # root로 접속

# ────────── 로그 ──────────
docker logs mycontainer                   # 로그 전체
docker logs -f mycontainer                # 실시간 로그 (follow)
docker logs --tail 100 mycontainer        # 마지막 100줄
docker logs --since 2024-01-01 mycontainer
docker logs --timestamps mycontainer      # 타임스탬프 포함

# ────────── 상세 정보 ──────────
docker inspect mycontainer                # 전체 JSON 정보
docker inspect --format '{{.NetworkSettings.IPAddress}}' mycontainer
docker inspect --format '{{json .Config.Env}}' mycontainer

# ────────── 리소스 모니터링 ──────────
docker stats                              # 실시간 리소스 사용량
docker stats --no-stream                  # 1회 스냅샷
docker stats mycontainer

# ────────── 프로세스 ──────────
docker top mycontainer                    # 컨테이너 내 프로세스 목록

# ────────── 파일 복사 ──────────
docker cp mycontainer:/app/log.txt ./     # 컨테이너 → 호스트
docker cp ./config.json mycontainer:/app/ # 호스트 → 컨테이너

# ────────── 삭제 ──────────
docker rm mycontainer                     # 컨테이너 삭제 (정지 후)
docker rm -f mycontainer                  # 강제 삭제 (실행 중도 가능)
docker container prune                    # 정지된 컨테이너 전부 삭제
```

---

### 📌 포트 포워딩 (-p)

```bash
# 형식: -p [호스트IP:]호스트포트:컨테이너포트[/프로토콜]

docker run -p 80:80 nginx                 # 80→80
docker run -p 8080:80 nginx               # 호스트 8080 → 컨테이너 80
docker run -p 127.0.0.1:8080:80 nginx     # 로컬호스트만 허용
docker run -p 80:80/tcp -p 53:53/udp ...  # 프로토콜 지정
docker run -P nginx                       # EXPOSE된 포트 랜덤 할당

# 포트 확인
docker port mycontainer
docker inspect -f '{{json .NetworkSettings.Ports}}' mycontainer
```

```
호스트                  컨테이너
0.0.0.0:8080  ──────►  :80 (Nginx)
```

---

### 📌 환경변수 주입

```bash
# -e 플래그
docker run -e NODE_ENV=production -e PORT=3000 myapp

# --env-file (권장, 민감 정보 관리)
docker run --env-file .env myapp

# .env 파일 예시
NODE_ENV=production
DB_HOST=db.example.com
DB_PASSWORD=secret123
```

```dockerfile
# Dockerfile에서 기본값 설정
ENV NODE_ENV=development
ENV PORT=3000
# → docker run -e NODE_ENV=production 으로 덮어쓰기 가능
```

---

### 📌 리소스 제한

```bash
# CPU 제한
docker run --cpus="0.5" myapp           # 0.5 코어
docker run --cpus="2" myapp             # 2 코어
docker run --cpu-shares=512 myapp       # 상대적 가중치 (기본 1024)

# 메모리 제한
docker run -m 512m myapp                # 최대 512MB
docker run -m 1g myapp                  # 최대 1GB
docker run --memory=512m --memory-swap=1g myapp  # swap 포함 1GB

# 확인
docker inspect mycontainer | grep -i memory
docker stats mycontainer
```

```bash
# 리소스 초과 시 동작
# 메모리 초과 → OOM Killer에 의해 강제 종료 (exit 137)
# CPU는 초과해도 스로틀링만 (종료 안 됨)
```

---

### 📌 재시작 정책 (--restart)

```bash
docker run --restart=no myapp              # 기본값, 재시작 안 함
docker run --restart=always myapp          # 항상 재시작 (수동 stop 제외)
docker run --restart=unless-stopped myapp  # docker stop 전까지 항상 재시작
docker run --restart=on-failure myapp      # 비정상 종료(exit != 0)시만
docker run --restart=on-failure:3 myapp    # 최대 3번 재시도
```

| 정책 | 설명 | 권장 상황 |
|------|------|----------|
| `no` | 재시작 안 함 | 일회성 작업 |
| `always` | 항상 재시작 | 데몬 서비스 |
| `unless-stopped` | 수동 stop 제외 재시작 | 프로덕션 서비스 |
| `on-failure` | 오류 시만 재시작 | 실패 허용 작업 |

---

### 📌 헬스체크 (HEALTHCHECK)

```dockerfile
# Dockerfile에서 정의
HEALTHCHECK --interval=30s \   # 체크 간격
            --timeout=10s \    # 타임아웃
            --start-period=5s \# 초기 대기 시간
            --retries=3 \      # 실패 허용 횟수
  CMD curl -f http://localhost:3000/health || exit 1
```

```bash
# 실행 시 옵션으로도 지정 가능
docker run \
  --health-cmd="curl -f http://localhost/health || exit 1" \
  --health-interval=30s \
  --health-timeout=5s \
  --health-retries=3 \
  myapp

# 상태 확인
docker inspect --format='{{.State.Health.Status}}' mycontainer
# starting | healthy | unhealthy

docker ps  # STATUS 컬럼에서 확인
# Up 2 minutes (healthy)
# Up 2 minutes (unhealthy)
```

---

### 📌 로깅 드라이버

```bash
# 로깅 드라이버 종류
# json-file (기본) → /var/lib/docker/containers/[id]/[id]-json.log
# syslog           → 시스템 syslog
# journald         → systemd journal
# fluentd          → Fluentd 집계 서버
# awslogs          → AWS CloudWatch
# gelf             → Graylog

# 로깅 드라이버 지정
docker run --log-driver=json-file \
           --log-opt max-size=10m \
           --log-opt max-file=3 \
           myapp

docker run --log-driver=syslog \
           --log-opt syslog-address=tcp://192.168.1.100:514 \
           myapp

docker run --log-driver=fluentd \
           --log-opt fluentd-address=localhost:24224 \
           myapp

# 데몬 전역 설정: /etc/docker/daemon.json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

---

### 📌 디버깅 방법

```bash
# 1. 로그 확인
docker logs -f --tail 50 mycontainer

# 2. 컨테이너 내부 접속
docker exec -it mycontainer sh     # Alpine (bash 없을 수 있음)
docker exec -it mycontainer bash

# 3. 파일 시스템 확인
docker exec mycontainer ls -la /app
docker exec mycontainer cat /app/config.json

# 4. 프로세스 확인
docker top mycontainer
docker exec mycontainer ps aux

# 5. 네트워크 확인
docker exec mycontainer netstat -tulnp
docker exec mycontainer curl localhost:3000/health

# 6. 종료된 컨테이너 디버깅
docker run --entrypoint sh myapp      # ENTRYPOINT 오버라이드

# 7. 임시 디버깅 컨테이너 (같은 네임스페이스)
docker run --rm -it \
  --pid container:mycontainer \
  --network container:mycontainer \
  nicolaka/netshoot                   # 네트워크 디버깅 툴

# 8. 이벤트 모니터링
docker events                         # 실시간 도커 이벤트
docker events --filter container=mycontainer
```

---

## 4️⃣ 볼륨 (Volume)

### 📌 데이터 영속성 이해

```
컨테이너 특성:
- 컨테이너 삭제 → Writable Layer 삭제 → 데이터 사라짐
- 컨테이너는 기본적으로 Stateless (무상태)

영속성이 필요한 경우:
- 데이터베이스 (MySQL, PostgreSQL, MongoDB)
- 로그 파일
- 업로드 파일
- 설정 파일
```

```
볼륨 없음:                 볼륨 있음:
Container                  Container
├─ Writable Layer          ├─ Writable Layer
│  └─ /var/lib/mysql  ←X  └─ /var/lib/mysql ───► Volume (Host)
└─ Image Layers               삭제해도 Volume 유지!
삭제 시 데이터 소멸
```

---

### 📌 볼륨 종류

#### 1. Named Volume (도커 관리)
```bash
# 가장 권장되는 방식
docker volume create mydata
docker run -v mydata:/var/lib/mysql mysql

# 특징:
# - Docker가 /var/lib/docker/volumes/에 저장
# - 컨테이너 삭제 후에도 볼륨 유지
# - 이름으로 관리 편리
# - 백업/복원 용이
```

#### 2. Bind Mount (호스트 경로 직접 마운트)
```bash
# 호스트의 특정 경로를 컨테이너에 마운트
docker run -v /host/path:/container/path myapp
docker run -v $(pwd):/app myapp               # 현재 디렉토리
docker run -v $(pwd)/config:/app/config:ro myapp  # 읽기 전용

# 특징:
# - 개발 시 코드 실시간 반영 (Hot Reload)
# - 호스트 경로 의존적
# - 권한 문제 주의 (uid/gid)
```

#### 3. tmpfs Mount (메모리)
```bash
# 메모리에만 존재, 컨테이너 종료 시 사라짐
docker run --tmpfs /tmp:rw,size=100m myapp
docker run --mount type=tmpfs,dst=/tmp,tmpfs-size=100m myapp

# 특징:
# - 매우 빠름 (메모리 I/O)
# - 민감한 임시 데이터 (비밀번호 등)
# - 컨테이너 간 공유 불가
```

#### 비교표

| 구분 | Named Volume | Bind Mount | tmpfs |
|------|-------------|------------|-------|
| 저장 위치 | Docker 관리 영역 | 호스트 임의 경로 | 메모리 |
| 영속성 | ✅ | ✅ | ❌ |
| 성능 | 높음 | 중간 | 최고 |
| 이식성 | 높음 | 낮음 | N/A |
| 용도 | DB, 영속 데이터 | 개발, 설정 파일 | 임시 데이터 |

---

### 📌 볼륨 명령어

```bash
# 볼륨 생성
docker volume create mydata
docker volume create --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.1.1,rw \
  --opt device=:/path/to/dir \
  nfs-volume

# 볼륨 목록
docker volume ls
docker volume ls -f dangling=true        # 사용되지 않는 볼륨

# 볼륨 상세 정보
docker volume inspect mydata

# 볼륨 삭제
docker volume rm mydata
docker volume prune                      # 미사용 볼륨 전부 삭제

# 볼륨과 함께 컨테이너 실행
docker run -v mydata:/data myapp
docker run --mount source=mydata,target=/data myapp  # 명시적 문법
```

---

### 📌 컨테이너 간 볼륨 공유

```bash
# Named Volume으로 공유
docker run -d --name db -v shareddata:/data mysql
docker run -d --name app -v shareddata:/data myapp

# --volumes-from (다른 컨테이너 볼륨 상속)
docker run --name datacontainer -v /data busybox
docker run --volumes-from datacontainer ubuntu
docker run --volumes-from datacontainer --name backup ubuntu
```

---

### 📌 볼륨 백업 / 복원

```bash
# 백업
docker run --rm \
  -v mydata:/data \          # 백업할 볼륨 마운트
  -v $(pwd):/backup \        # 호스트 백업 경로
  ubuntu \
  tar czf /backup/backup.tar.gz -C /data .

# 복원
docker run --rm \
  -v mydata:/data \
  -v $(pwd):/backup \
  ubuntu \
  bash -c "cd /data && tar xzf /backup/backup.tar.gz"
```

---

### 📌 실전 예제

#### DB 데이터 보존 (MySQL)
```bash
docker run -d \
  --name mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=mydb \
  -v mysql-data:/var/lib/mysql \    # 데이터 보존
  -v $(pwd)/init.sql:/docker-entrypoint-initdb.d/init.sql \  # 초기화 SQL
  -p 3306:3306 \
  mysql:8.0
```

#### 읽기 전용 설정 파일 마운트
```bash
docker run -d \
  --name nginx \
  -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \  # 읽기 전용
  -v nginx-logs:/var/log/nginx \                    # 로그는 쓰기 가능
  -p 80:80 \
  nginx
```

---

## 5️⃣ 네트워크 (Network)

### 📌 도커 네트워크 동작 원리

```
도커 설치 시 자동 생성되는 네트워크:
- bridge (docker0): 기본 브릿지 네트워크
- host: 호스트 네트워크 직접 사용
- none: 네트워크 없음

동작 원리:
호스트 (eth0: 192.168.1.100)
│
├── docker0 (172.17.0.1) ← 가상 브릿지
│     ├── veth0 ─── container1 (172.17.0.2)
│     ├── veth1 ─── container2 (172.17.0.3)
│     └── veth2 ─── container3 (172.17.0.4)
│
└── 외부 통신: NAT (iptables MASQUERADE)
```

---

### 📌 네트워크 드라이버 종류

#### 1. bridge (기본, 단일 호스트)
```bash
# 기본 bridge 네트워크로 실행
docker run nginx                    # docker0 브릿지에 연결

# 사용자 정의 bridge (권장)
docker network create mynetwork
docker run --network mynetwork nginx

# 특징:
# - 같은 호스트 내 컨테이너 간 통신
# - NAT를 통한 외부 통신
# - 기본 bridge: DNS 이름 해석 안 됨
# - 사용자 정의 bridge: DNS 이름 해석 됨 ✅
```

#### 2. host (성능 최우선)
```bash
docker run --network host nginx

# 특징:
# - 호스트 네트워크 스택 직접 사용
# - 포트 포워딩 불필요 (호스트 포트 = 컨테이너 포트)
# - 최고 성능
# - Linux에서만 완전 동작 (Mac/Windows: 제한적)
# - 격리 없음 → 보안 주의
```

#### 3. none (완전 격리)
```bash
docker run --network none myapp

# 특징:
# - 네트워크 인터페이스 없음 (loopback만)
# - 외부 통신 완전 차단
# - 보안이 중요한 배치 작업
```

#### 4. overlay (멀티 호스트, Swarm)
```bash
# Docker Swarm 필요
docker network create --driver overlay myoverlay

# 특징:
# - 여러 Docker 호스트 간 컨테이너 통신
# - VXLAN 기반 터널링
# - Swarm / Kubernetes에서 사용
```

#### 5. macvlan
```bash
docker network create \
  --driver macvlan \
  --subnet 192.168.1.0/24 \
  --gateway 192.168.1.1 \
  -o parent=eth0 \
  macvlan-net

# 특징:
# - 컨테이너가 물리 네트워크에 직접 연결
# - MAC 주소 할당
# - 고성능, 레거시 앱 통합
```

---

### 📌 네트워크 명령어

```bash
# 네트워크 목록
docker network ls

# 네트워크 생성
docker network create mynet
docker network create \
  --driver bridge \
  --subnet 172.20.0.0/16 \
  --ip-range 172.20.10.0/24 \
  --gateway 172.20.10.1 \
  mynet

# 네트워크 상세 정보
docker network inspect mynet

# 컨테이너를 네트워크에 연결/해제
docker network connect mynet mycontainer
docker network disconnect mynet mycontainer

# 네트워크 삭제
docker network rm mynet
docker network prune    # 사용 안 하는 네트워크 삭제
```

---

### 📌 컨테이너 간 통신 & 사용자 정의 네트워크

```bash
# 기본 bridge vs 사용자 정의 bridge 차이
# 기본 bridge:
docker run --name app1 myapp
docker run --name app2 myapp
# app1 → app2 통신: IP 주소로만 가능 (172.17.0.x)

# 사용자 정의 bridge (권장):
docker network create appnet
docker run --network appnet --name app1 myapp
docker run --network appnet --name app2 myapp
# app1 → app2 통신: 컨테이너 이름으로 가능!
# ping app2, curl http://app2:3000 ✅

# 컨테이너가 여러 네트워크에 연결
docker network connect frontend app1   # 추가 연결
docker network connect backend app1
```

---

### 📌 DNS 기반 통신

```bash
# 사용자 정의 네트워크에서는 자동 DNS 설정
docker network create mynet
docker run -d --network mynet --name db postgres
docker run -d --network mynet --name app myapp

# app 컨테이너에서 db 접근
# 환경변수: DB_HOST=db  (IP가 아닌 이름!)
docker exec app curl http://db:5432

# Docker 내장 DNS 서버: 127.0.0.11
# /etc/resolv.conf 에서 확인 가능
docker exec app cat /etc/resolv.conf
# nameserver 127.0.0.11

# 네트워크 별칭 (alias)
docker run --network mynet \
           --network-alias=database \
           --name postgres-primary \
           postgres
# "database"로 접근 가능
```

---

### 📌 EXPOSE vs 포트 바인딩 (-p)

```dockerfile
# Dockerfile의 EXPOSE
EXPOSE 3000
# → 문서화 목적 (실제로 포트를 열지 않음)
# → 컨테이너 내부 포트 명시
```

```bash
# -p : 실제 포트 바인딩 (호스트 ↔ 컨테이너)
docker run -p 8080:3000 myapp      # 호스트:8080 → 컨테이너:3000
docker run -p 3000:3000 myapp      # 동일 포트
docker run -P myapp                # EXPOSE된 포트 랜덤 바인딩

# 차이 정리:
# EXPOSE 3000          → "이 컨테이너는 3000 포트 사용"이라고 선언
# -p 8080:3000         → 실제로 호스트 8080을 컨테이너 3000으로 연결
# 같은 네트워크 컨테이너끼리는 -p 없이 EXPOSE된 포트로 직접 통신 가능
```

---

### 📌 실전 예제: 웹서버 ↔ DB 연결

```bash
# 1. 전용 네트워크 생성
docker network create webapp-net

# 2. DB 컨테이너 실행 (외부 포트 바인딩 없이)
docker run -d \
  --name postgres \
  --network webapp-net \
  -e POSTGRES_PASSWORD=secret \
  -e POSTGRES_DB=mydb \
  -v pgdata:/var/lib/postgresql/data \
  postgres:15

# 3. 웹 앱 컨테이너 실행
docker run -d \
  --name webapp \
  --network webapp-net \
  -e DATABASE_URL=postgresql://postgres:secret@postgres:5432/mydb \
  -p 80:3000 \
  mywebapp

# 결과:
# webapp → postgres : 컨테이너 이름(DNS)으로 내부 통신
# 외부 → webapp    : 80포트로 접근 가능
# 외부 → postgres  : 직접 접근 불가 (보안 ✅)
```
## 6️⃣ Docker Compose

### 📌 Compose 개념

```
Docker Compose = 멀티 컨테이너 앱을 YAML로 선언적 관리

단일 명령으로:
- 여러 컨테이너 동시 실행
- 네트워크 자동 생성
- 볼륨 자동 관리
- 의존성 순서 제어
```

```bash
# 설치 확인
docker compose version         # v2 (현재 주류, 플러그인 방식)
docker-compose version         # v1 (별도 바이너리, 구식)
```

---

### 📌 docker-compose.yml 구조

```yaml
# compose.yml (또는 docker-compose.yml)

version: "3.9"           # Compose 파일 버전 (v3.x 권장)

services:                # 컨테이너 정의
  webapp:
    ...
  db:
    ...

networks:                # 네트워크 정의
  mynet:
    ...

volumes:                 # 볼륨 정의
  mydata:
    ...
```

---

### 📌 주요 키워드 상세

```yaml
version: "3.9"

services:

  # ── 앱 서비스 ──────────────────────────────────────
  app:
    # 이미지 지정 (빌드 대신)
    image: node:20-alpine

    # 또는 빌드 (이미지 대신)
    build:
      context: .                   # Dockerfile 위치
      dockerfile: Dockerfile.prod  # 파일명 지정
      args:
        NODE_ENV: production
      target: runner               # 멀티스테이지 대상

    # 컨테이너 이름
    container_name: myapp

    # 환경변수
    environment:
      NODE_ENV: production
      PORT: 3000
    env_file:
      - .env
      - .env.production

    # 포트 바인딩
    ports:
      - "80:3000"
      - "443:443"

    # 볼륨
    volumes:
      - ./src:/app/src              # Bind Mount
      - node_modules:/app/node_modules  # Named Volume

    # 네트워크
    networks:
      - frontend
      - backend

    # 의존성 (시작 순서)
    depends_on:
      db:
        condition: service_healthy   # 헬스체크 통과 후 시작
      redis:
        condition: service_started   # 시작만 확인

    # 재시작 정책
    restart: unless-stopped

    # 리소스 제한
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
        reservations:
          cpus: '0.25'
          memory: 256M

    # 헬스체크
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 10s

    # 실행 명령 덮어쓰기
    command: ["node", "server.js"]
    entrypoint: ["./docker-entrypoint.sh"]

    # 로깅
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"

    # 레이블
    labels:
      - "traefik.enable=true"

    # 추가 호스트 (/etc/hosts)
    extra_hosts:
      - "myhost:192.168.1.100"

    # stdin, tty
    stdin_open: true
    tty: true

  # ── DB 서비스 ──────────────────────────────────────
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: ${DB_PASSWORD}   # .env 참조
    volumes:
      - pgdata:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    networks:
      - backend
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user -d mydb"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

  # ── Redis 서비스 ───────────────────────────────────
  redis:
    image: redis:7-alpine
    command: redis-server --requirepass ${REDIS_PASSWORD}
    volumes:
      - redisdata:/data
    networks:
      - backend

  # ── Nginx 리버스 프록시 ────────────────────────────
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./ssl:/etc/nginx/ssl:ro
    depends_on:
      - app
    networks:
      - frontend

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
    internal: true           # 외부 연결 차단

volumes:
  pgdata:
  redisdata:
  node_modules:
```

---

### 📌 Compose 명령어

```bash
# ── 기본 ──────────────────────────────────────────
docker compose up                     # 서비스 시작 (포그라운드)
docker compose up -d                  # 백그라운드 실행
docker compose up --build             # 이미지 재빌드 후 시작
docker compose up --build --force-recreate  # 강제 재생성

docker compose down                   # 컨테이너 + 네트워크 삭제
docker compose down -v                # 볼륨까지 삭제 (데이터 삭제!)
docker compose down --rmi all         # 이미지까지 삭제

# ── 상태 확인 ──────────────────────────────────────
docker compose ps                     # 서비스 상태
docker compose ps -a                  # 종료된 것 포함

# ── 로그 ───────────────────────────────────────────
docker compose logs                   # 전체 로그
docker compose logs -f                # 실시간 로그
docker compose logs -f app            # 특정 서비스
docker compose logs --tail 50 app

# ── 명령 실행 ──────────────────────────────────────
docker compose exec app bash          # 실행 중인 컨테이너 접속
docker compose exec db psql -U user mydb
docker compose run --rm app npm test  # 일회성 실행

# ── 빌드 ───────────────────────────────────────────
docker compose build                  # 전체 빌드
docker compose build app              # 특정 서비스 빌드
docker compose build --no-cache app

# ── 스케일 ─────────────────────────────────────────
docker compose up -d --scale app=3   # 인스턴스 3개 실행

# ── 일시정지 ───────────────────────────────────────
docker compose pause app
docker compose unpause app
docker compose stop app
docker compose start app
docker compose restart app
```

---

### 📌 환경변수 관리 (.env)

```bash
# .env 파일 (자동으로 읽힘)
DB_PASSWORD=secret123
REDIS_PASSWORD=redis456
NODE_ENV=production
```

```yaml
# compose.yml에서 참조
services:
  db:
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}     # .env 참조
      POSTGRES_USER: ${DB_USER:-defaultuser}  # 기본값 지정
```

```bash
# 여러 env 파일 지정
docker compose --env-file .env.prod up -d

# 환경변수 확인
docker compose config        # 변수 치환 후 최종 설정 확인
```

---

### 📌 Compose 파일 오버라이드 (dev / prod 분리)

```yaml
# docker-compose.yml (공통 베이스)
services:
  app:
    image: myapp
    environment:
      NODE_ENV: production

# docker-compose.override.yml (개발용, 자동 적용)
services:
  app:
    build: .
    environment:
      NODE_ENV: development
    volumes:
      - .:/app                 # 소스 마운트 (Hot Reload)
    ports:
      - "3000:3000"
    command: npm run dev

# docker-compose.prod.yml (프로덕션용)
services:
  app:
    restart: always
    deploy:
      resources:
        limits:
          memory: 1G
```

```bash
# 개발 환경 (override 자동 적용)
docker compose up -d

# 프로덕션 환경 (명시적으로 파일 지정)
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

---

### 📌 Compose v2 vs v3 차이

| 구분 | v2 | v3 |
|------|----|----|
| 대상 | 단일 호스트 | Swarm 지원 |
| `deploy` 키 | ❌ | ✅ (Swarm 전용) |
| 메모리 제한 | `mem_limit` | `deploy.resources` |
| `depends_on` | 단순 순서 | condition 지원 (v3.9+) |
| 현재 권장 | compose v2 CLI | 동일 |

> 💡 현재는 `version` 키 자체가 deprecated. Compose CLI v2에서는 버전 관계없이 통합 지원.

---

## 7️⃣ 보안 (Security)

### 📌 컨테이너 보안 기본

```
컨테이너 보안 위협:
1. 이미지 취약점 (CVE)
2. 설정 오류 (root 실행, 과도한 권한)
3. 비밀 정보 노출 (환경변수, 이미지에 하드코딩)
4. 네트워크 노출 (불필요한 포트)
5. Supply Chain 공격 (악성 베이스 이미지)

보안 원칙:
- 최소 권한 원칙 (Principle of Least Privilege)
- 신뢰할 수 있는 이미지만 사용
- 정기적인 취약점 스캔
- 컨테이너 불변성 유지
```

---

### 📌 root vs non-root 실행

```dockerfile
# ❌ 위험: root로 실행 (기본값)
FROM node:20
WORKDIR /app
COPY . .
CMD ["node", "server.js"]   # root로 실행됨

# ✅ 안전: non-root 유저 생성 및 사용
FROM node:20-alpine

WORKDIR /app

# 전용 유저/그룹 생성
RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup

# 파일 권한 설정
COPY --chown=appuser:appgroup . .

RUN npm ci --only=production

# 유저 전환
USER appuser

CMD ["node", "server.js"]
```

```bash
# 실행 시 유저 지정
docker run --user 1000:1000 myapp
docker run --user nobody myapp
```

---

### 📌 읽기 전용 컨테이너

```bash
# 파일시스템 읽기 전용으로 실행
docker run --read-only myapp

# 쓰기가 필요한 디렉토리만 tmpfs로 허용
docker run --read-only \
  --tmpfs /tmp \
  --tmpfs /var/run \
  myapp
```

```yaml
# Compose에서
services:
  app:
    read_only: true
    tmpfs:
      - /tmp
      - /var/run
```

---

### 📌 Capability 제한

```bash
# Linux Capability: 루트 권한을 세분화한 것
# 기본적으로 컨테이너는 일부 capability만 가짐

# 모든 capability 제거 후 필요한 것만 추가 (최소 권한)
docker run \
  --cap-drop=ALL \           # 전부 제거
  --cap-add=NET_BIND_SERVICE \  # 1024 이하 포트 바인딩
  --cap-add=CHOWN \             # 파일 소유자 변경
  myapp

# 주요 Capability
# NET_BIND_SERVICE : 1024 이하 포트 바인딩
# CHOWN           : chown 명령
# SETUID/SETGID   : uid/gid 변경
# SYS_PTRACE      : 프로세스 추적 (디버깅)
# NET_ADMIN       : 네트워크 설정 변경
```

---

### 📌 Seccomp / AppArmor

```bash
# Seccomp: 허용할 시스템 콜 제한
# 기본 프로파일은 Docker가 자동 적용 (300여개 syscall 차단)

# 기본 프로파일 (대부분의 경우 충분)
docker run --security-opt seccomp=/etc/docker/seccomp.json myapp

# Seccomp 비활성화 (위험)
docker run --security-opt seccomp=unconfined myapp

# AppArmor (Ubuntu/Debian): MAC(Mandatory Access Control)
docker run --security-opt apparmor=docker-default myapp
docker run --security-opt apparmor=myprofile myapp

# 조합 사용
docker run \
  --cap-drop=ALL \
  --cap-add=NET_BIND_SERVICE \
  --read-only \
  --security-opt seccomp=/etc/docker/seccomp.json \
  myapp
```

---

### 📌 시크릿 관리 및 환경변수 노출 방지

```bash
# ❌ 위험: 이미지에 직접 포함
ENV DB_PASSWORD=secret123         # 이미지 히스토리에 노출!

# ❌ 위험: 빌드 ARG로 전달
ARG DB_PASSWORD                   # docker history에 노출!
RUN echo $DB_PASSWORD > /secret

# ✅ 런타임 환경변수 (최소한의 방법)
docker run -e DB_PASSWORD=secret myapp

# ✅ 파일로 마운트 (권장)
docker run -v /host/secrets:/run/secrets:ro myapp
# 앱에서 파일 읽기: fs.readFileSync('/run/secrets/db_password')

# ✅ Docker Secrets (Swarm)
echo "mypassword" | docker secret create db_password -
docker service create \
  --secret db_password \
  myapp
# 컨테이너 내 /run/secrets/db_password 파일로 주입

# ✅ BuildKit 시크릿 (빌드 중 시크릿, 이미지에 남지 않음)
docker build \
  --secret id=npmrc,src=$HOME/.npmrc \
  -t myapp .
```

```dockerfile
# BuildKit 시크릿 사용 예시
# syntax=docker/dockerfile:1
FROM node:20
RUN --mount=type=secret,id=npmrc,target=/root/.npmrc \
    npm install
# .npmrc는 이미지에 포함되지 않음!
```

#### .gitignore & .dockerignore 체크
```
# .dockerignore 필수 항목
.env
.env.*
*.pem
*.key
secrets/
```

---

### 📌 Docker Bench for Security

```bash
# CIS Docker Benchmark 기반 보안 점검 자동화
docker run -it --net host --pid host --userns host --cap-add audit_control \
  -e DOCKER_CONTENT_TRUST=$DOCKER_CONTENT_TRUST \
  -v /etc:/etc:ro \
  -v /usr/bin/containerd:/usr/bin/containerd:ro \
  -v /usr/bin/runc:/usr/bin/runc:ro \
  -v /usr/lib/systemd:/usr/lib/systemd:ro \
  -v /var/lib:/var/lib:ro \
  -v /var/run/docker.sock:/var/run/docker.sock:ro \
  --label docker_bench_security \
  docker/docker-bench-security

# 점검 항목:
# [PASS] [WARN] [INFO] [NOTE] 등으로 결과 출력
# - 호스트 설정
# - Docker 데몬 설정
# - Docker 데몬 파일
# - 컨테이너 이미지
# - 컨테이너 런타임
# - Docker Swarm 설정
```

---

## 8️⃣ 실전 / 운영 (Practice & Ops)

### 📌 이미지 경량화 & 빌드 캐시 최적화

```dockerfile
# ── 경량화 기법 ──────────────────────────────────

# 1. 베이스 이미지 선택
FROM node:20-alpine    # 가벼운 베이스

# 2. 레이어 최소화 (RUN 명령 합치기)
# ❌ 레이어 낭비
RUN apt update
RUN apt install -y curl
RUN apt clean

# ✅ 하나로 합치고 캐시 정리
RUN apt update && \
    apt install -y curl && \
    rm -rf /var/lib/apt/lists/*

# 3. 멀티스테이지 빌드 (앞서 설명)

# 4. .dockerignore로 컨텍스트 크기 축소

# 5. 불필요한 파일 제거
RUN pip install -r requirements.txt && \
    pip cache purge && \
    find /usr -type d -name '__pycache__' -exec rm -rf {} + 2>/dev/null || true
```

```bash
# ── 빌드 캐시 최적화 ─────────────────────────────

# 캐시 활용 확인
docker build --progress=plain -t myapp .  # 상세 빌드 로그

# BuildKit 사용 (기본 활성화, 병렬 빌드)
DOCKER_BUILDKIT=1 docker build -t myapp .

# 원격 캐시 활용 (CI/CD)
docker buildx build \
  --cache-from type=registry,ref=myregistry/myapp:cache \
  --cache-to type=registry,ref=myregistry/myapp:cache,mode=max \
  -t myapp:latest .

# 이미지 레이어 분석
docker history myapp:latest
docker inspect myapp:latest

# dive 툴로 레이어 상세 분석
dive myapp:latest
```

---

### 📌 CI/CD 파이프라인 연동 (GitHub Actions)

```yaml
# .github/workflows/docker.yml

name: Docker Build & Push

on:
  push:
    branches: [main]
    tags: ['v*']
  pull_request:
    branches: [main]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      # Docker Buildx 설정 (멀티플랫폼)
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      # GHCR 로그인
      - name: Log in to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      # 메타데이터 추출 (태그 자동 생성)
      - name: Extract Docker metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=ref,event=branch
            type=ref,event=pr
            type=semver,pattern={{version}}
            type=semver,pattern={{major}}.{{minor}}
            type=sha,prefix=git-

      # 빌드 & 푸시 (캐시 활용)
      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          platforms: linux/amd64,linux/arm64  # 멀티 플랫폼
          cache-from: type=gha
          cache-to: type=gha,mode=max

      # 취약점 스캔
      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ steps.meta.outputs.version }}
          format: sarif
          output: trivy-results.sarif
          severity: CRITICAL,HIGH
```

---

### 📌 Private Registry 구축

```bash
# ── Docker Hub Private Repo ────────────────────────
# Docker Hub에서 Private 레포 생성 후 사용

# ── 셀프 호스팅 Registry ─────────────────────────
# 기본 registry 실행
docker run -d \
  --name registry \
  -p 5000:5000 \
  -v registry-data:/var/lib/registry \
  --restart always \
  registry:2

# Push / Pull
docker tag myapp localhost:5000/myapp:1.0
docker push localhost:5000/myapp:1.0
docker pull localhost:5000/myapp:1.0

# 이미지 목록 API
curl http://localhost:5000/v2/_catalog
curl http://localhost:5000/v2/myapp/tags/list

# TLS 설정 (프로덕션 필수)
docker run -d \
  --name registry \
  -p 443:443 \
  -v $(pwd)/certs:/certs \
  -v registry-data:/var/lib/registry \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  registry:2

# ── Harbor (엔터프라이즈 프라이빗 레지스트리) ──────
# RBAC, 취약점 스캔, 이미지 서명, 복제 지원
# https://goharbor.io
```

---

### 📌 도커 모니터링 (Prometheus + Grafana)

```yaml
# docker-compose.monitoring.yml

services:
  # Docker 메트릭 수집 (cAdvisor)
  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
    ports:
      - "8080:8080"

  # 메트릭 저장 (Prometheus)
  prometheus:
    image: prom/prometheus:latest
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-data:/prometheus
    ports:
      - "9090:9090"
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.retention.time=15d'

  # 대시보드 (Grafana)
  grafana:
    image: grafana/grafana:latest
    volumes:
      - grafana-data:/var/lib/grafana
    ports:
      - "3000:3000"
    environment:
      GF_SECURITY_ADMIN_PASSWORD: admin123
    depends_on:
      - prometheus

volumes:
  prometheus-data:
  grafana-data:
```

```yaml
# prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor:8080']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

```bash
# 주요 모니터링 지표
# container_cpu_usage_seconds_total     : CPU 사용량
# container_memory_usage_bytes          : 메모리 사용량
# container_network_receive_bytes_total : 네트워크 수신
# container_fs_reads_bytes_total        : 디스크 I/O

# Docker 데몬 메트릭 활성화
# /etc/docker/daemon.json
{
  "metrics-addr": "0.0.0.0:9323",
  "experimental": true
}
```

---

### 📌 스토리지 드라이버

| 드라이버 | 커널 | 성능 | 안정성 | 사용 환경 |
|---------|------|------|--------|----------|
| `overlay2` | 3.18+ | ✅ 높음 | ✅ 높음 | 기본 권장 (ext4, xfs) |
| `devicemapper` | - | 중간 | 중간 | 구식, 지양 |
| `btrfs` | - | 높음 | 중간 | btrfs 파일시스템 |
| `zfs` | - | 높음 | 높음 | zfs 파일시스템 |
| `vfs` | 무관 | ❌ 낮음 | 높음 | 테스트용만 |

```bash
# 현재 스토리지 드라이버 확인
docker info | grep "Storage Driver"

# /etc/docker/daemon.json 으로 변경
{
  "storage-driver": "overlay2"
}
```

---

### 📌 Docker Swarm 기초

```bash
# ── 초기화 ──────────────────────────────────────
# 매니저 노드 초기화
docker swarm init --advertise-addr 192.168.1.100

# 워커 노드 참가 (init 시 출력된 토큰 사용)
docker swarm join \
  --token SWMTKN-1-xxx \
  192.168.1.100:2377

# ── 노드 관리 ────────────────────────────────────
docker node ls                          # 노드 목록
docker node inspect node1
docker node update --availability drain node1  # 점검 모드

# ── 서비스 관리 ──────────────────────────────────
# 서비스 생성 (컨테이너 3개 복제)
docker service create \
  --name webapp \
  --replicas 3 \
  --publish published=80,target=3000 \
  --network myoverlay \
  myapp:1.0

# 서비스 목록
docker service ls
docker service ps webapp        # 각 태스크 상태

# 스케일링
docker service scale webapp=5

# 롤링 업데이트
docker service update \
  --image myapp:2.0 \
  --update-parallelism 2 \
  --update-delay 30s \
  webapp

# 롤백
docker service rollback webapp

# 서비스 삭제
docker service rm webapp

# ── 스택 (Compose 파일로 배포) ────────────────────
docker stack deploy -c docker-compose.prod.yml mystack
docker stack ls
docker stack services mystack
docker stack rm mystack
```

---

### 📌 Kubernetes 전환 개념

```
Docker (단일 호스트)       Kubernetes (멀티 호스트 클러스터)
────────────────────       ─────────────────────────────────
컨테이너                →   Pod (1개 이상의 컨테이너 그룹)
docker run              →   kubectl run / Deployment
docker-compose          →   Deployment + Service + Ingress
Named Volume            →   PersistentVolume (PV/PVC)
docker network          →   Service (ClusterIP/NodePort/LoadBalancer)
환경변수                 →   ConfigMap / Secret
--restart               →   ReplicaSet (자동 복구)
docker service          →   Deployment (롤링 업데이트)
Docker Hub              →   Container Registry (ECR, GCR, ACR)
```

```yaml
# Docker Compose → Kubernetes 변환 예시

# compose.yml
services:
  webapp:
    image: myapp:1.0
    ports: ["80:3000"]
    environment:
      DB_HOST: db

# → k8s Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: myapp:1.0
        ports:
        - containerPort: 3000
        env:
        - name: DB_HOST
          value: "db-service"   # K8s Service 이름

# → k8s Service
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  selector:
    app: webapp
  ports:
  - port: 80
    targetPort: 3000
  type: LoadBalancer
```

```bash
# Kompose: Compose → K8s 자동 변환
kompose convert -f docker-compose.yml

# 주요 K8s 개념
# Pod          : 최소 배포 단위 (Docker 컨테이너 ≈)
# Deployment   : Pod 복제 및 업데이트 관리
# Service      : Pod 네트워크 노출 (로드밸런서)
# Ingress      : HTTP/HTTPS 라우팅 (Nginx 같은 역할)
# ConfigMap    : 설정 데이터
# Secret       : 민감 데이터
# PersistentVolume : 영속 스토리지
# Namespace    : 논리적 클러스터 분리
# Helm         : K8s 패키지 매니저 (Docker Compose 역할)
```

---

## 📎 빠른 참조 (Cheat Sheet)

```bash
# ── 이미지 ─────────────────────────────────────
docker images                          # 이미지 목록
docker pull nginx:alpine               # 다운로드
docker push myrepo/myapp:1.0           # 업로드
docker build -t myapp:1.0 .            # 빌드
docker tag myapp:1.0 myapp:latest      # 태그
docker rmi myapp:1.0                   # 삭제
docker image prune -a                  # 미사용 전부 삭제
docker history myapp                   # 레이어 히스토리

# ── 컨테이너 ────────────────────────────────────
docker run -d -p 80:80 --name web nginx  # 실행
docker ps -a                             # 목록
docker stop web && docker rm web         # 정지 후 삭제
docker rm -f web                         # 강제 삭제
docker exec -it web bash                 # 접속
docker logs -f web                       # 로그
docker stats                             # 리소스 모니터링
docker inspect web                       # 상세 정보
docker cp web:/etc/nginx/nginx.conf .    # 파일 복사

# ── 볼륨 ────────────────────────────────────────
docker volume create mydata
docker volume ls
docker volume inspect mydata
docker volume rm mydata
docker volume prune

# ── 네트워크 ─────────────────────────────────────
docker network create mynet
docker network ls
docker network inspect mynet
docker network connect mynet web
docker network rm mynet

# ── Compose ──────────────────────────────────────
docker compose up -d
docker compose down -v
docker compose ps
docker compose logs -f
docker compose exec app bash
docker compose build --no-cache

# ── 시스템 정리 ──────────────────────────────────
docker system df                         # 디스크 사용량
docker system prune                      # 미사용 리소스 정리
docker system prune -a --volumes         # 전부 정리 (주의!)
```