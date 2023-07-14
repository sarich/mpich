FROM ubuntu:latest
RUN apt-get update && apt-get install -y \
    build-essential \
    gfortran \
    emacs-nox \
    wget \
    sudo \
    git \
    libtool \
    automake \
    autoconf \
    gdb \
    strace \
    python3
RUN echo -e '\
#!/bin/bash\n\
https://github.com/pmodels/mpich.git \n\
cd mpich \n\
git submodule update --init --recursive \n\
./autogen.sh \n\
./configure --prefix=$PWD/install --with-device=ch3:sock \n\
make \n\
make install \n\
./install/bin/mpiexec -n 2 ./examples/cpi \n\
cd test/mpi/attr \n\
make testing > build.script \'
> /usr/bin/build_me.sh
CMD ["/usr/bin/build_me.sh"]
