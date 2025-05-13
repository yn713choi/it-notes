FROM rockylinux:9

# 보안 설정 및 시스템 강화
RUN dnf update -y --nobest && \
    dnf install -y sudo && \
    # 불필요한 서비스/패키지 제거
    dnf remove -y telnet rsh talk cups && \
    # 필수 보안 패키지 설치
    dnf install -y policycoreutils-python-utils selinux-policy-targeted

# 로컬 유저 생성 및 sudo 권한 부여
# RUN useradd -m -s /bin/bash localuser && \
#     echo "localuser ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/localuser && \
#     chmod 0440 /etc/sudoers.d/localuser

# 기본 패키지 업데이트 및 필수 도구 설치
RUN dnf update -y --nobest && \
    dnf install -y git vim unzip tar jq gcc python3 python3-pip && \
    dnf clean all

# AWS CLI 설치 (최신 v2)
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install && \
    rm -rf aws awscliv2.zip

# AWS Session Manager Plugin 설치
RUN curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_64bit/session-manager-plugin.rpm" -o "session-manager-plugin.rpm" && \
    dnf install -y ./session-manager-plugin.rpm && \
    rm -f session-manager-plugin.rpm

# gossm 설치 (최신 안정 버전)
RUN curl -LO "https://github.com/gjbae1212/gossm/releases/download/v1.4.1/gossm_1.4.1_Linux_x86_64.tar.gz" && \
    tar -xzf gossm_1.4.1_Linux_x86_64.tar.gz -C /usr/local/bin gossm && \
    chmod +x /usr/local/bin/gossm && \
    rm -f gossm_1.4.1_Linux_x86_64.tar.gz

# Node.js 18 LTS 설치
RUN curl -fsSL https://rpm.nodesource.com/setup_18.x | bash - && \
    dnf install -y nodejs --nogpgcheck && \
    dnf clean all

# Terraform 설치 (최신 안정 버전)
RUN curl -fsSL "https://releases.hashicorp.com/terraform/1.7.3/terraform_1.7.3_linux_amd64.zip" -o "terraform.zip" && \
    unzip terraform.zip -d /usr/local/bin && \
    rm -f terraform.zip

# AWS CDK 설치 (최신 v2)
RUN npm install -g aws-cdk

# Python 및 관련 패키지 설치
RUN pip3 install --upgrade pip && \
    pip3 install --no-cache-dir boto3 awscli-local

# kubectl 설치 (최신 안정 버전)
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/

# k9s 설치 (최신 안정 버전)
RUN curl -fsSL "https://github.com/derailed/k9s/releases/latest/download/k9s_Linux_amd64.tar.gz" -o "k9s.tar.gz" && \
    tar -xzf k9s.tar.gz -C /usr/local/bin k9s && \
    rm -f k9s.tar.gz

# Helm 설치 (최신 안정 버전)
RUN curl -fsSL "https://get.helm.sh/helm-v3.17.3-linux-amd64.tar.gz" -o "helm.tar.gz" && \
    tar -xzf helm.tar.gz && \
    mv linux-amd64/helm /usr/local/bin/helm && \
    chmod +x /usr/local/bin/helm && \
    rm -rf helm.tar.gz linux-amd64

# 보안 설정
RUN chmod 600 /etc/shadow && \
    chmod 600 /etc/gshadow && \
    chmod 644 /etc/passwd && \
    chmod 644 /etc/group 
    # 디렉토리 권한 설정
    # chmod 755 /home/localuser && \
    # chown -R localuser:localuser /home/localuser

# 환경 변수 설정
ENV PATH="/usr/local/bin:$PATH"

# 사용자 전환
# USER localuser
# WORKDIR /home/localuser
# -v 볼륨 붙일때 권한 이슈로 보류

# 기본 실행 쉘
CMD ["/bin/bash"]

