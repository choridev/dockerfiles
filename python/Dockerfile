# 1. Use Python as base image (Debian)
FROM python:3.12.5

# 2. Update packages and install required packages
RUN apt-get update && apt-get install -y \
    zsh \
    # locales \
    vim \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# # 3. Set locale (Korean UTF-8 support)
# RUN echo "ko_KR.UTF-8 UTF-8" >> /etc/locale.gen && \
#     locale-gen

# # 4. Set environment variables (locale)
# ENV LANG=ko_KR.UTF-8
# ENV LANGUAGE=ko_KR:ko

# 5. Set timezone (Korean time)
RUN ln -fs /usr/share/zoneinfo/Asia/Seoul /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# 6. Configure Git default branch and editor settings
RUN git config --global core.editor vim
RUN git config --global init.defaultBranch main

# 7. Install Oh My Zsh and plugins
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && \
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# 8. Add plugins
RUN sed -i 's/^plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' ~/.zshrc

# 9. Change zsh theme to zhann
RUN sed -i 's/^ZSH_THEME=.*/ZSH_THEME="zhann"/' ~/.zshrc

# 10. Set working directory
WORKDIR /workspace

# 11. Change default shell to zsh
RUN chsh -s $(which zsh)

# 12. Keep container alive
CMD ["sleep", "infinity"]
