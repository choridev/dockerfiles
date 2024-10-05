# 1. 베이스 이미지로 eclipse-temurin 사용 (Ubuntu)
FROM eclipse-temurin:17

# 2. 패키지 업데이트 및 필요 패키지 설치
RUN apt-get update && apt-get install -y \
    zsh \
    language-pack-ko \
    locales \
    vim \
    git \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 3. 로케일 설정 (한글 UTF-8 지원)
RUN locale-gen ko_KR.UTF-8

# 4. 환경 변수 설정 (로케일)
ENV LANG=ko_KR.UTF-8
ENV LANGUAGE=ko_KR:ko
ENV LC_ALL=ko_KR.UTF-8

# 5. 시간대 설정 (한국 시간)
RUN ln -fs /usr/share/zoneinfo/Asia/Seoul /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# 6. Git 기본 브랜치와 에디터 관련 설정
RUN git config --global core.editor vim
RUN git config --global init.defaultBranch main

# 7. Oh My Zsh 및 플러그인 설치
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && \
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# 8. zsh 설정: 플러그인 추가
RUN sed -i 's/^plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' ~/.zshrc

# 9. 작업 디렉토리 설정
WORKDIR /workspace

# 10. 기본 셸을 zsh로 변경
RUN chsh -s $(which zsh) 

# 11. zsh를 기본 셸로 설정
ENTRYPOINT ["zsh"]