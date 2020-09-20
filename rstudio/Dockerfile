FROM rocker/tidyverse

MAINTAINER Alfredo Garbuno Iñigo "alfredo.garbuno@gmail.com"

RUN git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh \
    && cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

CMD ["/bin/zsh"]
