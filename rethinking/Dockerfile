FROM rocker/r-apt:xenial

ARG RSTUDIO_DEB=rstudio-xenial-1.1.383-amd64.deb

RUN apt-get update && apt-get -y install vim gdebi-core

RUN wget https://raw.githubusercontent.com/hvariant/dotfiles/master/vimrc_minimal -O /root/.vimrc

# Config for rstudio user
RUN mkdir -p $HOME/.R/ \
    && echo "\nCXXFLAGS=-O3 -mtune=native -march=native -Wno-unused-variable -Wno-unused-function \n" >> $HOME/.R/Makevars \
    && echo "\nCXXFLAGS+=-flto -ffat-lto-objects  -Wno-unused-local-typedefs \n" >> $HOME/.R/Makevars \
    && echo "rstan::rstan_options(auto_write = TRUE)\n" >> $HOME/.Rprofile \
    && echo "options(mc.cores = parallel::detectCores())\n" >> $HOME/.Rprofile

RUN install.r --deps rstan coda mvtnorm devtools loo remotes bayesplot rstanarm rstantools shinystan ggmcmc \
    && installGithub.r rmcelreath/rethinking

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y libglu1 libxcomposite-dev libxslt1-dev \
    lsb-release xorg xauth && cd /root/ && wget "https://download1.rstudio.org/$RSTUDIO_DEB" \
    && gdebi -n $RSTUDIO_DEB && rm $RSTUDIO_DEB

RUN apt-get update && apt-get -y install tmux

WORKDIR /root/
