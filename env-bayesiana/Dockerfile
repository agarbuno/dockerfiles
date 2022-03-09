FROM rocker/verse:4.1.1
MAINTAINER Alfredo Garbuno Iñigo "alfredo.garbuno@itam.mx"

# Declaramos al usuario, y las carpetas de trabajo
ENV RSTUDIO_USER rstudio
ENV TARGET_DIR documentos
ENV CMDSTAN_VER 2.28.2

RUN adduser $RSTUDIO_USER sudo

# Declaramos ruta de trabajo
WORKDIR /home/$RSTUDIO_USER/

# Instalamos herramientas: htop (monitoreo) y librerias que necesita R para
# compilar paquetes
RUN apt-get install --no-install-recommends -qq wget ca-certificates make g++

RUN apt-get update \
    && apt-get install -y libmagick++-dev htop

# Instalamos cmdstan de manera manual 
RUN mkdir -p .cmdstan
WORKDIR /home/$RSTUDIO_USER/.cmdstan
RUN wget --progress=dot:mega https://github.com/stan-dev/cmdstan/releases/download/v$CMDSTAN_VER/cmdstan-$CMDSTAN_VER.tar.gz
RUN tar -zxpf cmdstan-$CMDSTAN_VER.tar.gz \
    && rm cmdstan-$CMDSTAN_VER.tar.gz
RUN cd cmdstan-$CMDSTAN_VER; make build
WORKDIR /home/$RSTUDIO_USER/

# Aseguramos que podemos trabajar desde ~/rstudio/home
RUN mkdir /home/$RSTUDIO_USER/$TARGET_DIR \
    && chown -R $RSTUDIO_USER:staff /home/$RSTUDIO_USER/

VOLUME [ "/home/$RSTUDIO_USER/$TARGET_DIR" ]

RUN chmod -R 775 /home/$RSTUDIO_USER/$TARGET_DIR \
    && chmod -R g+s /home/$RSTUDIO_USER/$TARGET_DIR

CMD ["/init"]
