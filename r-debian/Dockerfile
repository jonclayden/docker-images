FROM debian

RUN apt update && apt upgrade -y
RUN apt install -y r-base-dev
RUN apt install -y man sudo git vim less

RUN useradd -G sudo -m user && echo "user:docker" | chpasswd
WORKDIR /home/user
USER user

RUN mkdir Rlibrary
RUN echo 'options(repos=c(CRAN="https://cran.rstudio.com"), menu.graphics=FALSE, Ncpus=2L); .libPaths(c("/home/user/Rlibrary",.libPaths()))' >.Rprofile

CMD [ "/bin/bash" ]
