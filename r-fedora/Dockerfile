FROM fedora

RUN yum update -y
RUN yum install -y R
RUN yum install -y libXt-devel
RUN yum install -y sudo

RUN useradd -G wheel user && echo "user:docker" | chpasswd
WORKDIR /home/user
USER user

RUN mkdir Rlibrary
RUN echo 'options(repos=c(CRAN="https://cran.rstudio.com"), menu.graphics=FALSE, Ncpus=2L); .libPaths(c("/home/user/Rlibrary",.libPaths()))' >.Rprofile

CMD [ "/bin/bash" ]
