# Dockerfile 주요 변경사항

## 1. 보안 강화
- 불필요한 서비스/패키지 제거
  ```bash
  dnf remove -y telnet rsh talk cups
  ```
- SELinux 관련 패키지 설치
  ```bash
  dnf install -y policycoreutils-python-utils selinux-policy-targeted
  ```
- 중요 파일 권한 설정
  ```bash
  chmod 600 /etc/shadow
  chmod 600 /etc/gshadow
  chmod 644 /etc/passwd
  chmod 644 /etc/group
  ```
- 사용자 디렉토리 권한 설정
  ```bash
  chmod 755 /home/localuser
  chown -R localuser:localuser /home/localuser
  ```

## 2. 버전 업데이트
- Node.js 18 LTS 버전
- Terraform 1.7.3 (최신 안정 버전)
- AWS CLI v2 (최신 버전)
- kubectl (최신 안정 버전)
- k9s (최신 안정 버전)
- gossm 1.4.1 (최신 안정 버전)

## 3. 사용자 관리
- localuser 생성 및 sudo 권한 부여
  ```bash
  useradd -m -s /bin/bash localuser
  echo "localuser ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/localuser
  chmod 0440 /etc/sudoers.d/localuser
  ```
- 컨테이너 실행 시 localuser로 전환
  ```dockerfile
  USER localuser
  WORKDIR /home/localuser
  ```

## 4. 캐시 정리 및 최적화
- 패키지 설치 후 캐시 정리
  ```bash
  dnf clean all
  ```
- 임시 파일 제거
  ```bash
  rm -rf aws awscliv2.zip
  rm -f terraform.zip
  rm -f k9s.tar.gz
  ```
