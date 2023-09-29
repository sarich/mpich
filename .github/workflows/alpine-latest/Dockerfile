FROM alpine:latest
RUN apk update && apk add --no-cache  \
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
    python3 \
    patch \
    alpine-sdk \
    bash \
    linux-headers

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
	 "cp summary.junit.xml /results\n" > /usr/bin/build_me.sh

RUN echo "#!/bin/bash -x\n" \
	 "cp /results/placeholder /results/summary.junit.xml\n" \
	 "cat /results/summary.junit.xml" > /usr/bin/build_me_dryrun.sh

RUN chmod +x /usr/bin/build_me.sh	 
CMD ["/usr/bin/build_me.sh"]
