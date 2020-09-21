FROM rocker/tidyverse
MAINTAINER Alfredo Garbuno Iñigo "alfredo.garbuno@gmail.com"

# Declaramos al usuario (por referencia pues rocker define al usuario como rstudio)
ENV RSTUDIO_USER rstudio

# RUN adduser $RSTUDIO_USER sudo
# VOLUME [ "/home/$RSTUDIO_USER/notas-fundamentos" ]

WORKDIR /home/$RSTUDIO_USER/

RUN apt update \
    && apt install -y libmagick++-dev htop

USER $RSTUDIO_USER

VOLUME [ "/home/rstudio/notas-fundamentos" ]

RUN chown -R rstudio:rstudio /home/$RSTUDIO_USER/notas-fundamentos/

RUN R -e "install.packages('renv', repos = 'http://cran.us.r-project.org', type = 'source')" \
    # && R -e "install.packages('rmarkdown', repos = 'http://cran.us.r-project.org', type = 'source')" \
    # && R -e "install.packages('knitr', repos = 'http://cran.us.r-project.org', type = 'source')" \
    && git clone https://github.com/tereom/fundamentos.git notas-fundamentos \
    && cd notas-fundamentos \
    && git checkout 'ag/docker-image' \
    # && git checkout 'dev' \
    && git pull \
    # && apt update \
    && R -e "renv::restore(project = '/home/rstudio/notas-fundamentos/')" 

# TODO: Add volume for development ~/.local/share/renv

# RUN git clone https://github.com/tereom/fundamentos.git notas-fundamentos

# RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.1/zsh-in-docker.sh)" -- \
#     -t https://github.com/denysdovhan/spaceship-prompt \
#     -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
#     -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
#     -p git

# ENV TERM xterm-256color
# ENV POWERLEVEL9K_ALWAYS_SHOW_CONTEXT=true
# COPY .p10k.zsh /root/
# RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.1/zsh-in-docker.sh)" -- \
#     -p git
# RUN echo 'POWERLEVEL9K_MODE="awesome-patched"' >> /root/.zshrc
# RUN echo '[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh' >> /root/.zshrc
#
# CMD ["/usr/bin/zsh"]
# CMD ["/bin/zsh"]

USER root

CMD ["/init"]
