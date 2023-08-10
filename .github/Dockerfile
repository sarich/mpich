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
RUN mkdir /results
RUN echo "placeholder text" > /results/placeholder
RUN echo "#!/bin/bash -x\n" \
         "git clone https://github.com/pmodels/mpich.git\n" \
         "cd mpich\n" \
	 "git submodule update --init --recursive\n" \
	 "./autogen.sh\n" \
	 "./configure --prefix=/install --with-device=ch3:sock\n" \
	 "make\n" \
	 "make install\n" \
	 "/install/bin/mpiexec -n 2 ./examples/cpi\n" \
	 "cd /mpich/test/mpi\n" \
	 "./configure --with-mpi=/install\n" \
	 "cd attr\n" \
	 "make testing | tee build.script\n" \
	 "cp summary.junit.xml /results" > /usr/bin/build_me.sh
RUN chmod +x /usr/bin/build_me.sh	 
CMD ["/usr/bin/build_me.sh"]
