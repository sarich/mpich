FROM fedora:latest
RUN dnf makecache --refresh && dnf -y install \
    gfortran \
    g++ \
    emacs-nox \
    hwloc-libs \
    wget \
    git \
    libtool \
    automake \
    autoconf \
    gdb \
    strace \
    python3 \
    which \
    perl-Time-HiRes
RUN dnf -y groupinstall "Development Tools"
RUN mkdir /results
RUN echo "placeholder text" > /results/placeholder
RUN echo -e "#!/bin/bash -x\n" \
         "git clone https://github.com/pmodels/mpich.git\n" \
         "cd mpich\n" \
	 "git submodule update --init --recursive\n" \
	 "./autogen.sh\n" \
	 "./configure --prefix=/install\n" \
	 "make -j 6\n" \
	 "make install\n" \
	 "/install/bin/mpiexec -n 2 ./examples/cpi\n" \
	 "cd /mpich/test/mpi\n" \
	 "./configure --with-mpi=/install\n" \
	 "cd attr\n" \
	 "make testing | tee build.script\n" \
	 "cp summary.junit.xml /results" > /usr/bin/build_me.sh

RUN echo -e "#!/bin/bash -x\n" \
	 "cp /results/placeholder /results/summary.junit.xml\n" \
	 "cat /results/summary.junit.xml" > /usr/bin/build_me_dryrun.sh

RUN chmod +x /usr/bin/build_me.sh	 
CMD ["/usr/bin/build_me.sh"]
