# Docker file for JuliaBox APIs
# Version:42

FROM julialang/juliaboxpkgdist:v0.3.12

MAINTAINER Tanmay Mohapatra

RUN ln -fs /opt/julia/bin/julia /usr/bin/julia
RUN apt-get remove -y openmpi-*
RUN apt-get install -y mpich2

# add juser
# create group and user with specific ids if required on systems where the user running juliabox is not the first user
RUN groupadd juser \
    && useradd -m -d /home/juser -s /bin/bash -g juser -G staff juser \
    && echo "export HOME=/home/juser" >> /home/juser/.bashrc


# add Julia v0.4 build
RUN mkdir -p /opt/julia-0.4.6 && \
    curl -s -L https://julialang.s3.amazonaws.com/bin/linux/x64/0.4/julia-0.4.6-linux-x86_64.tar.gz | tar -C /opt/julia-0.4.6 -x -z --strip-components=1 -f -
RUN ln -fs /opt/julia-0.4.6 /opt/julia-0.4


# Make Julia 0.4 the default
RUN rm /opt/julia && ln -fs /opt/julia-0.4.6 /opt/julia

USER juser
ENV HOME /home/juser
ENV PATH /usr/local/texlive/2014/bin/x86_64-linux:/usr/local/bin:/usr/bin:/bin:/sbin:/usr/sbin:/opt/julia/bin
ENV PIPS_NLP_PAR_SHARED_LIB /opt/juliabox/.juliabox/workspace/PIPS/build_pips/PIPS-NLP/libparpipsnlp.so
ENV PIPS_NLP_SHARED_LIB /opt/juliabox/.juliabox/workspace/PIPS/build_pips/PIPS-NLP/libpipsnlp.so
WORKDIR /home/juser

RUN /opt/julia-0.4/bin/julia -e "(Pkg.installed(\"JuliaWebAPI\") == nothing) && Pkg.add(\"JuliaWebAPI\")"

ENTRYPOINT ["julia", "-e", "using JuliaWebAPI; using Compat; process();"]
