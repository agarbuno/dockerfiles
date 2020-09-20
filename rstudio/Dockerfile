FROM rocker/tidyverse

MAINTAINER Alfredo Garbuno Iñigo "alfredo.garbuno@gmail.com"

ENV TERM xterm-256color
ENV POWERLEVEL9K_ALWAYS_SHOW_CONTEXT=true

WORKDIR /home/fundamentos/

RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.1/zsh-in-docker.sh)" -- \
    -t https://github.com/denysdovhan/spaceship-prompt \
    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
    -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
    -p git

# COPY .p10k.zsh /root/
# RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.1/zsh-in-docker.sh)" -- \
#     -p git
# RUN echo 'POWERLEVEL9K_MODE="awesome-patched"' >> /root/.zshrc
# RUN echo '[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh' >> /root/.zshrc
#
# CMD ["/usr/bin/zsh"]
CMD ["/bin/zsh"]
