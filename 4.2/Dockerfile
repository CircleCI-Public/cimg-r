# vim:set ft=dockerfile:

# Do not edit individual Dockerfiles manually. Instead, please make changes to the Dockerfile.template, which will be used by the build script to generate Dockerfiles.

FROM cimg/base:2022.04

LABEL maintainer="Community & Partner Engineering Team <community-partner@circleci.com>"

ENV R_VERSION="4.2.0"
ENV R_HOME=/usr/local/lib/R
# Define number of parallel processing cores
ENV NCPUS=${NCPUS:--1}
# Specify the repository for packages to be installed from
ENV CRAN=https://packagemanager.rstudio.com/cran/__linux__/focal/latest

LABEL maintainer="Community & Partner Engineering Team <community-partner@circleci.com>"

# Additional info https://cran.r-project.org/bin/linux/ubuntu/fullREADME.html
# Additional resources 
# https://cloud.r-project.org/src/base/R-4/

# Add necessary sources
RUN echo "deb http://mirror.math.princeton.edu/pub/ubuntu/ focal-backports main restricted universe" | sudo tee -a /etc/apt/sources.list && \
      curl https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc && \
      sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/" && \
      sudo add-apt-repository --enable-source --yes "ppa:marutter/rrutter4.0" && \
      sudo add-apt-repository --enable-source --yes "ppa:c2d4u.team/c2d4u4.0+"

# Install packages needed for R
RUN sudo apt-get update && sudo apt-get install -y --no-install-recommends \
      devscripts \
      dirmngr \
      ed \
      file \
      fonts-texgyre \
      default-jdk \
      r-cran-docopt \
      g++ \
      gfortran \
      ghostscript \
      gsfonts \
      # will likely split some of these by category for better organization
      # perhaps this should be included with shiny or other web-based R functionality instead
      # hugo \
      less \
      # libraries generally related to processing images and datatypes. Does XML also need to be included?
      # for xml/RDF im particular:
      # libxslt-dev \
      # librdf0 \
      # redland-utils \
      # rasqal-utils \
      # raptor2-utils \
      libblas-dev \
      libbz2-dev \
      libglpk-dev \
      libgmp3-dev \
      libhunspell-dev \
      libicu-dev \
      liblapack-dev \
      liblzma-dev \
      libopenblas0-pthread \
      libopenmpi-dev \
      libpcre2* \
      libjpeg-turbo* \
      libpangocairo-* \
      libpng16* \
      libpcre2-dev \
      libtiff* \
      liblzma* \
      libgit2-dev \
      # js library - not sure if included for a shiny variant 
      libv8-dev \
      # some packages here are used for conneting to databases, however, in a CI environment, i'm not entirely sure if
      # this is necessary, but including them for now. after all, R scripts could depend on the data it's working with
      # however, this can probably be user defined
      # bigrquery, RMySQL, RMariaDB, RPostgres, unixodbc-dev
      libxslt-dev \
      littler \
      locales \
      software-properties-common \
      unixodbc-dev \
      zlib1g && \
      curl https://cloud.r-project.org/src/base/R-4/R-${R_VERSION}.tar.gz -o R.tar.gz && \
      tar -xzf "R.tar.gz" && \
      sudo rm -rf /var/lib/apt/lists/*

# path for site library      
# /usr/local/lib/R/site-library
# link example littler scripts to PATH; likely not necessary??
RUN sudo ln -s /usr/lib/R/site-library/littler/examples/install.r /usr/local/bin/install.r && \
      sudo ln -s /usr/lib/R/site-library/littler/examples/install2.r /usr/local/bin/install2.r && \
      sudo ln -s /usr/lib/R/site-library/littler/examples/installBioc.r /usr/local/bin/installBioc.r && \
      sudo ln -s /usr/lib/R/site-library/littler/examples/installDeps.r /usr/local/bin/installDeps.r && \
      sudo ln -s /usr/lib/R/site-library/littler/examples/installGithub.r /usr/local/bin/installGithub.r && \
      sudo ln -s /usr/lib/R/site-library/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r && \
      sudo chmod +x /usr/lib/R/site-library/littler/examples/ && \
      sudo chmod a+w /usr/local/lib/R/site-library

RUN install2.r --error --skipinstalled -n "$NCPUS" \
    # tidyverse here includes code packaes like dplyr and ggplot2
    bigrquery \
    devtools \
    RMariaDB \
    RMySQL \
    RPostgres \
    tidyverse && \
    rm -rf /tmp/downloaded_packages