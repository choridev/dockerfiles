# 1. 베이스 이미지로 ubuntu 사용
FROM ubuntu:24.04

# 2. 패키지 업데이트 및 Terraform 설치 전 필요 패키지 설치
RUN apt-get update && apt-get install -y \
    wget \
    gpg \
    lsb-release

# 3. HashiCorp의 GPG 키 추가 및 Terraform 저장소 추가 (Ubuntu 24.04)
RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list

# 4. 패키지 목록 업데이트 후 필요한 패키지 설치 및 불필요한 파일 정리
RUN apt-get update && apt-get install -y \
    curl \
    zsh \
    language-pack-ko \
    locales \
    vim \
    git \
    terraform \
    unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 5. AWS CLI 설치
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install && \
    rm -rf awscliv2.zip aws/

# 6. 로케일 설정 (한글 UTF-8 지원)
RUN locale-gen ko_KR.UTF-8

# 7. 환경 변수 설정 (로케일)
ENV LANG=ko_KR.UTF-8
ENV LANGUAGE=ko_KR:ko
ENV LC_ALL=ko_KR.UTF-8

# 8. 시간대 설정 (한국 시간)
RUN ln -fs /usr/share/zoneinfo/Asia/Seoul /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# 9. Git 기본 브랜치와 에디터 관련 설정
RUN git config --global core.editor vim
RUN git config --global init.defaultBranch main

# 10. Oh My Zsh 및 플러그인 설치
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && \
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# 11. zsh 설정: 플러그인 추가
RUN sed -i 's/^plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' ~/.zshrc

# 12. zsh 테마를 ys로 변경
RUN sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="ys"/' ~/.zshrc

# 13. vim 설정 적용
RUN echo "set hlsearch\nset nu\nset ts=2\nset sts=2\nset sw=2\nset title\nset autoindent" > ~/.vimrc

# 14. 작업 디렉토리 설정
WORKDIR /workspace

# 15. 기본 셸을 zsh로 변경
RUN chsh -s $(which zsh) 

# 16. zsh를 기본 셸로 설정
ENTRYPOINT ["zsh"]
