FROM rocker/verse:4.1.1
MAINTAINER Alfredo Garbuno Iñigo "alfredo.garbuno@itam.mx"

# Declaramos al usuario, y las carpetas de trabajo
ENV RSTUDIO_USER rstudio
ENV TARGET_DIR documentos

RUN adduser $RSTUDIO_USER sudo

# Declaramos ruta de trabajo
WORKDIR /home/$RSTUDIO_USER/

# Instalamos herramientas: htop (monitoreo) y librerias que necesita R para
# compilar paquetes
RUN apt-get install --no-install-recommends -qq wget ca-certificates make g++

RUN apt-get update \
    && apt-get install -y libmagick++-dev htop

    # Aseguramos que podemos trabajar desde ~/rstudio/home
RUN mkdir /home/$RSTUDIO_USER/$TARGET_DIR \
    && chown -R $RSTUDIO_USER:staff /home/$RSTUDIO_USER/

VOLUME [ "/home/$RSTUDIO_USER/$TARGET_DIR" ]

RUN chmod -R 775 /home/$RSTUDIO_USER/$TARGET_DIR \
    && chmod -R g+s /home/$RSTUDIO_USER/$TARGET_DIR

CMD ["/init"]
