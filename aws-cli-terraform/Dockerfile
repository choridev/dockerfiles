# 1. Use Ubuntu as base image
FROM ubuntu:24.04

# 2. Update packages and install required packages for Terraform
RUN apt-get update && apt-get install -y \
    wget \
    gpg \
    lsb-release

# 3. Add HashiCorp's GPG key and Terraform repository (Ubuntu 24.04)
RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list

# 4. Update package list, install required packages and clean up
RUN apt-get update && apt-get install -y \
    curl \
    zsh \
    # language-pack-ko \
    # locales \
    vim \
    git \
    terraform \
    unzip \
    jq \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 5. Install AWS CLI
RUN ARCH=$(uname -m) && \
    if [ "$ARCH" = "x86_64" ]; then \
        AWS_CLI_URL="https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip"; \
    elif [ "$ARCH" = "aarch64" ]; then \
        AWS_CLI_URL="https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip"; \
    else \
        echo "Unsupported architecture: $ARCH" && exit 1; \
    fi && \
    curl "$AWS_CLI_URL" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install && \
    rm -rf awscliv2.zip aws/

# # 6. Set locale (Korean UTF-8 support)
# RUN locale-gen ko_KR.UTF-8

# # 7. Set environment variables (locale)
# ENV LANG=ko_KR.UTF-8
# ENV LANGUAGE=ko_KR:ko
# ENV LC_ALL=ko_KR.UTF-8

# 8. Set timezone (Korean time)
RUN ln -fs /usr/share/zoneinfo/Asia/Seoul /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# 9. Configure Git default branch and editor settings
RUN git config --global core.editor vim
RUN git config --global init.defaultBranch main

# 10. Install Oh My Zsh and plugins
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && \
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# 11. Add plugins
RUN sed -i 's/^plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' ~/.zshrc

# 12. Change zsh theme to zhann
RUN sed -i 's/^ZSH_THEME=.*/ZSH_THEME="zhann"/' ~/.zshrc

# 13. Apply vim settings
RUN echo "set hlsearch\nset nu\nset ts=2\nset sts=2\nset sw=2\nset title\nset autoindent" > ~/.vimrc

# 14. Set working directory
WORKDIR /workspace

# 15. Change default shell to zsh
RUN chsh -s $(which zsh) 

# 16. Set zsh as default shell
ENTRYPOINT ["zsh"]
