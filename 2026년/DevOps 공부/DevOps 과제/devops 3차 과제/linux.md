# Linux

## 1. /etc 디렉토리 핵심 파일

- /etc : 시스템 전체 설정 파일들이 모여있는 디렉토리

- /etc/passwd : 사용자 계정 정보 (이름, UID, GID, 홈 디렉토리, 쉘)
- /etc/shadow : 사용자 암호화된 비밀번호 저장 (passwd에서 분리된 보안 파일)
- /etc/group : 그룹 정보 (그룹명, GID, 멤버 목록)
- /etc/hosts : IP 주소와 호스트명 매핑 (DNS보다 먼저 참조)
- /etc/resolv.conf : DNS 서버 주소 설정
- /etc/hostname : 현재 시스템의 호스트명
- /etc/fstab : 부팅 시 자동 마운트할 파일시스템 목록
- /etc/sudoers : sudo 권한 설정
- /etc/crontab : 시스템 예약 작업 설정
- /etc/ssh/sshd_config : SSH 서버 설정

### DNS 동작 흐름

1. 브라우저에 도메인 입력 (예: google.com)
2. 브라우저 캐시 확인 → 있으면 바로 사용
3. /etc/hosts 파일 확인 → 있으면 바로 사용
4. /etc/resolv.conf 에서 DNS 서버 주소 확인
5. DNS 서버(Resolver)에 쿼리 전송
6. Resolver → Root DNS 서버 → TLD DNS 서버 → 권한 DNS 서버 순으로 질의
7. IP 주소 응답받아 캐시에 저장 후 접속

### hosts vs DNS 차이

- /etc/hosts : 로컬 파일, DNS보다 먼저 참조, 수동으로 직접 작성
- DNS : 네트워크 서버, hosts에 없을 때 참조, 자동으로 전 세계 도메인 조회 가능
- 우선순위 : hosts 파일 → DNS 서버
- hosts는 소규모/테스트 환경, DNS는 실제 인터넷 환경에서 사용

## 2. 네트워크 설정 파일

- /etc/resolv.conf : DNS 서버 IP 주소 및 검색 도메인 설정
- /etc/network/interfaces : 네트워크 인터페이스 설정 (Debian 계열)
- /etc/sysconfig/network-scripts/ : 네트워크 인터페이스 설정 (RHEL 계열)
- /etc/hosts : IP ↔ 호스트명 매핑
- /etc/nsswitch.conf : 이름 해석 순서 설정 (hosts, dns 순서 지정)

### UID / GID 개념

- UID (User ID) : 사용자를 식별하는 고유 숫자
- GID (Group ID) : 그룹을 식별하는 고유 숫자
- 리눅스는 내부적으로 사용자명이 아닌 UID/GID로 권한을 관리

- UID 0 : root (최고 관리자)
- UID 1 ~ 999 : 시스템 계정 (서비스용)
- UID 1000 이상 : 일반 사용자 계정

- /etc/passwd 에서 확인 가능
- 예: `username:x:1001:1001::/home/username:/bin/bash'
  - 세 번째 값 = UID, 네 번째 값 = GID

### root 계정 특징

- UID = 0, GID = 0
- 시스템의 모든 파일, 디렉토리, 프로세스에 접근 가능
- 파일 권한(읽기/쓰기/실행) 제한을 받지 않음
- 홈 디렉토리는 /root (일반 사용자는 /home/이름)
- 기본 쉘은 /bin/bash
- 잘못 사용하면 시스템 전체에 영향 → 일반적으로 sudo로 대체해서 사용
- 원격 SSH 접속 시 root 직접 로그인은 보안상 비활성화 권장 (/etc/ssh/sshd_config 에서 설정)
