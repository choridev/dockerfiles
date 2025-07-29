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
    vim \
    git \
    terraform \
    python3 \
    python3-pip \
    golang-go \
    zip \
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

# 6. Install Session Manager Plugin for ECS Exec
RUN ARCH=$(uname -m) && \
    if [ "$ARCH" = "x86_64" ]; then \
        SSM_PLUGIN_URL="https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb"; \
    elif [ "$ARCH" = "aarch64" ]; then \
        SSM_PLUGIN_URL="https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_arm64/session-manager-plugin.deb"; \
    else \
        echo "Unsupported architecture for Session Manager Plugin: $ARCH" && exit 1; \
    fi && \
    curl -L "$SSM_PLUGIN_URL" -o "session-manager-plugin.deb" && \
    dpkg -i session-manager-plugin.deb && \
    rm session-manager-plugin.deb

# 7. Set timezone (Korean time)
RUN ln -fs /usr/share/zoneinfo/Asia/Seoul /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# 8. Configure Git default branch and editor settings
RUN git config --global core.editor vim
RUN git config --global init.defaultBranch main

# 9. Install Oh My Zsh and plugins
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && \
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# 10. Add plugins
RUN sed -i 's/^plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' ~/.zshrc

# 11. Change zsh theme to zhann
RUN sed -i 's/^ZSH_THEME=.*/ZSH_THEME="zhann"/' ~/.zshrc

# 12. Apply vim settings
RUN echo "set hlsearch\nset nu\nset ts=2\nset sts=2\nset sw=2\nset title\nset autoindent" > ~/.vimrc

# 13. Set working directory
WORKDIR /workspace

# 14. Change default shell to zsh
RUN chsh -s $(which zsh) 

# 15. Keep container alive
CMD ["sleep", "infinity"]
