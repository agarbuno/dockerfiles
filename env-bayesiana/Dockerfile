FROM rocker/verse:4.1.1
MAINTAINER Alfredo Garbuno Iñigo "alfredo.garbuno@itam.mx"

# Declaramos al usuario, y las
ENV RSTUDIO_USER rstudio
ENV TARGET_DIR documentos

RUN adduser $RSTUDIO_USER sudo

# Declaramos ruta de trabajo
WORKDIR /home/$RSTUDIO_USER/

# Instalamos herramientas: htop (monitoreo) y librerias que necesita R para
# compilar paquetes
RUN apt-get update \
    && apt-get install -y libmagick++-dev htop

RUN apt-get install -y --no-install-recommends \
            ghostscript \
            lmodern \
            pandoc-citeproc \
            qpdf \
            r-cran-formatr \
            r-cran-ggplot2 \
            r-cran-knitr \
            r-cran-rmarkdown \
            r-cran-runit \
            r-cran-testthat \
            texinfo \
            texlive-fonts-extra \
            texlive-fonts-recommended \
            texlive-latex-extra \
            texlive-latex-recommended \
            texlive-luatex \
            texlive-plain-generic \
            texlive-science \
            texlive-xetex

RUN mkdir ~/.R \
    && install2.r binb linl pinp tint \
    && echo _R_CHECK_FORCE_SUGGESTS_=FALSE > ~/.R/check.Renviron \
    && cd /usr/local/bin \
    && ln -s /usr/lib/R/site-library/littler/examples/render.r .

RUN tlmgr update --self &&\
    tlmgr init-usertree &&\
    tlmgr update --all &&\
# delete Tex Live sources and other potentially useless stuff
    echo "Delete TeX Live sources and other useless stuff." &&\
    (rm -rf /usr/share/texmf/source || true) &&\
    (rm -rf /usr/share/texlive/texmf-dist/source || true) &&\
    find /usr/share/texlive -type f -name "readme*.*" -delete &&\
    find /usr/share/texlive -type f -name "README*.*" -delete &&\
    (rm -rf /usr/share/texlive/release-texlive.txt || true) &&\
    (rm -rf /usr/share/texlive/doc.html || true) &&\
    (rm -rf /usr/share/texlive/index.html || true) &&\
# update font cache
    echo "Update font cache." &&\
    fc-cache -fv &&\
# clean up all temporary files
    echo "Clean up all temporary files." &&\
    apt-get clean -y &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -f /etc/ssh/ssh_host_* &&\
# delete man pages and documentation
    echo "Delete man pages and documentation." &&\
    rm -rf /usr/share/man &&\
    mkdir -p /usr/share/man &&\
    find /usr/share/doc -depth -type f ! -name copyright -delete &&\
    find /usr/share/doc -type f -name "*.pdf" -delete &&\
    find /usr/share/doc -type f -name "*.gz" -delete &&\
    find /usr/share/doc -type f -name "*.tex" -delete &&\
    (find /usr/share/doc -type d -empty -delete || true) &&\
    mkdir -p /usr/share/doc &&\
    rm -rf /var/cache/apt/archives &&\
    mkdir -p /var/cache/apt/archives &&\
    rm -rf /tmp/* /var/tmp/* &&\
    (find /usr/share/ -type f -empty -delete || true) &&\
    (find /usr/share/ -type d -empty -delete || true) &&\
    mkdir -p /usr/share/texmf/source &&\
    mkdir -p /usr/share/texlive/texmf-dist/source &&\
    echo "All done."

RUN echo "Installing cmdstan" \
    && git clone https://github.com/stan-dev/cmdstan.git .cmdstan --recursive \
    && cd .cmdstan \
    && make build

# Global site-wide config -- neeeded for building packages
RUN mkdir -p $HOME/.R/ \
    && echo "CXXFLAGS=-O3 -mtune=native -march=native -Wno-unused-variable -Wno-unused-function -flto -ffat-lto-objects  -Wno-unused-local-typedefs \n" >> $HOME/.R/Makevars

# Config for rstudio user
USER rstudio
RUN mkdir -p /home/rstudio/.R
RUN mkdir -p /home/rstudio/.local/share/rstudio
RUN touch /home/rstudio/.R/Makevars
RUN r -e 'cat("\nCXX14FLAGS=-O3 -march=native -mtune=native -fPIC", "CXX14=g++", file = "/home/rstudio/.R/Makevars", sep = "\n", append = TRUE)'
USER root

# Clonamos e inicializamos el ambiente
RUN mkdir /home/$RSTUDIO_USER/$TARGET_DIR \
    && chown -R $RSTUDIO_USER:staff /home/$RSTUDIO_USER/

VOLUME [ "/home/$RSTUDIO_USER/$TARGET_DIR" ]

RUN chmod -R 775 /home/$RSTUDIO_USER/$TARGET_DIR \
    && chmod -R g+s /home/$RSTUDIO_USER/$TARGET_DIR \
    && mkdir -p /home/$RSTUDIO_USER/.local/share/renv/ \
    && mkdir -p /home/$RSTUDIO_USER/.local/share/renv/source \
    && chown -R $RSTUDIO_USER:staff /home/$RSTUDIO_USER/.local/share/renv/source \
    && chmod -R 775 /home/$RSTUDIO_USER/.local/share/renv/source \
    && touch /home/$RSTUDIO_USER/.local/share/renv/projects \
    && chown -R $RSTUDIO_USER:staff /home/$RSTUDIO_USER/.local/share/renv/projects

CMD ["/init"]
