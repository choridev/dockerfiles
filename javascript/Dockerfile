# 1. 베이스 이미지로 node 사용 (Debian)
FROM node:20.17.0

# 2. 패키지 업데이트 및 필요 패키지 설치
RUN apt-get update && apt-get install -y \
    zsh \
    locales \
    vim \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 3. 로케일 설정 (한글 UTF-8 지원)
RUN echo "ko_KR.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen

# 4. 환경 변수 설정 (로케일)
ENV LANG=ko_KR.UTF-8
ENV LANGUAGE=ko_KR:ko

# 5. 시간대 설정 (한국 시간)
RUN ln -fs /usr/share/zoneinfo/Asia/Seoul /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# 6. Oh My Zsh 및 플러그인 설치
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && \
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# 7. zsh 설정: 플러그인 추가
RUN sed -i 's/^plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' ~/.zshrc

# 8. 작업 디렉토리 설정
WORKDIR /workspace

# 9. 기본 셸을 zsh로 변경
RUN chsh -s $(which zsh)

# 10. zsh를 기본 셸로 설정
ENTRYPOINT ["zsh"]