FROM fedora

RUN yum update -y
RUN yum install -y clang libcxx-devel libomp-devel gcc-gfortran readline-devel zlib-devel bzip2-devel xz-devel pcre2-devel libcurl-devel make valgrind-devel
RUN yum install -y findutils which sudo git

WORKDIR /root

RUN curl -O https://cloud.r-project.org/src/base-prerelease/R-devel.tar.gz
RUN tar -zxf R-devel.tar.gz && rm R-devel.tar.gz
COPY config.site R-devel/

RUN cd R-devel && ./configure --without-x --disable-java --with-valgrind-instrumentation=2 --with-system-valgrind-headers && make && make install
RUN rm -rf R-devel

RUN useradd -G wheel user && echo "user:docker" | chpasswd
USER user
WORKDIR /home/user

RUN mkdir Rlibrary
RUN echo 'options(repos=c(CRAN="https://cloud.r-project.org"), menu.graphics=FALSE, Ncpus=2L); .libPaths(c("/home/user/Rlibrary",.libPaths()))' >.Rprofile

CMD [ "/bin/bash" ]
