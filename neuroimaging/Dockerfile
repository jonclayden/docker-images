# Debian-based
FROM continuumio/miniconda3

# Set bash as the shell
SHELL ["/bin/bash", "-c"]

# Update basic packages and install system dependencies
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y sudo man gnupg cmake

# Set up Neurodebian
RUN wget -O- -q http://neuro.debian.net/lists/buster.de-md.full | tee /etc/apt/sources.list.d/neurodebian.sources.list
RUN apt-key adv --recv-keys --keyserver hkp://pool.sks-keyservers.net:80 0xA5D32F012649A5A9
RUN apt-get update

# Update Conda and create environment
RUN conda update -y conda
RUN conda create --name neuroimaging

# Install MRtrix3
RUN conda install -n neuroimaging -c mrtrix3 -y mrtrix3

# Install dcm2niix
RUN apt-get install -y dcm2niix

# Install FSL
# NB: This version from Neurodebian is not current, but using the proper installer doesn't work
RUN apt-get install -y fsl
# RUN wget -q https://fsl.fmrib.ox.ac.uk/fsldownloads/fslinstaller.py && python2 fslinstaller.py -d /opt/fsl

# Install TractoR
RUN conda install -n neuroimaging -y r
RUN wget -O- -q https://www.tractor-mri.org.uk/tractor.tar.gz | tar -C /opt -xzf -
RUN cd /opt/tractor && conda run -n neuroimaging make install

# Install NiftyReg
RUN git clone https://github.com/KCL-BMEIS/niftyreg.git
RUN mkdir -p niftyreg-build /opt/niftyreg
RUN cd niftyreg-build && conda run -n neuroimaging cmake -DCMAKE_INSTALL_PREFIX=/opt/niftyreg ../niftyreg && conda run -n neuroimaging make install
RUN rm -rf niftyreg niftyreg-build

# Install ANTs
# NB: This is extremely time-consuming and currently doesn't work, so skip for now
# RUN git clone https://github.com/ANTsX/ANTs.git
# RUN mkdir -p ANTs-build /opt/ants
# RUN cd ANTs-build && conda run -n neuroimaging cmake -DCMAKE_INSTALL_PREFIX=/opt/ants ../ANTs && conda run -n neuroimaging make
# RUN rm -rf ANTs ANTs-build

RUN useradd -G sudo user && echo "user:docker" | chpasswd
WORKDIR /home/user
USER user

COPY bashrc ./.bashrc
CMD [ "/bin/bash" ]
