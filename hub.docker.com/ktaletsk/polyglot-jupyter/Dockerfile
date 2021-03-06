ARG BASE_CONTAINER=jupyter/minimal-notebook
FROM $BASE_CONTAINER

MAINTAINER Konstantin Taletskiy <konstantin.taletskiy@axleinfo.com>

USER root

# R, Octave, Java, Scala
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ssh \
    fonts-dejavu \
    tzdata \
    gfortran \
    gcc \
    octave \
    openjdk-8-jdk \
    openjdk-11-jdk \
    libssl1.0.0 libssl-dev \
    scala \
    curl procps && \
    rm -rf /var/lib/apt/lists/* && \
    octave --eval 'pkg install -forge dataframe'
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

#Maven
ARG MAVEN_VERSION=3.6.0
ARG MAVEN_BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries
ARG MAVEN_SHA=fae9c12b570c3ba18116a4e26ea524b29f7279c17cbaadc3326ca72927368924d9131d11b9e851b8dc9162228b6fdea955446be41207a5cfc61283dd8a561d2f
RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
  && curl -fsSL -o /tmp/apache-maven.tar.gz ${MAVEN_BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  && echo "${MAVEN_SHA}  /tmp/apache-maven.tar.gz" | sha512sum -c - \
  && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
  && rm -f /tmp/apache-maven.tar.gz \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn
ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

COPY mvn-entrypoint.sh /usr/local/bin/mvn-entrypoint.sh
COPY settings-docker.xml /usr/share/maven/ref/

USER $NB_UID

RUN conda install --quiet --yes \
    'conda-forge::blas=*=openblas' \
    'ipywidgets=7.4*' \
    'pandas=0.23*' \
    'numexpr=2.6*' \
    'matplotlib=2.2*' \
    'scipy=1.1*' \
    'seaborn=0.9*' \
    'scikit-learn=0.20*' \
    'scikit-image=0.14*' \
    'sympy=1.1*' \
    'cython=0.28*' \
    'patsy=0.5*' \
    'statsmodels=0.9*' \
    'cloudpickle=0.5*' \
    'dill=0.2*' \
    'numba=0.38*' \
    'bokeh=0.13*' \
    'sqlalchemy=1.2*' \
    'hdf5=1.10*' \
    'h5py=2.7*' \
    'vincent=0.4.*' \
    'beautifulsoup4=4.6.*' \
    'protobuf=3.*' \
    'xlrd' \
    'rpy2=2.9*' \
    'r-base=3.5.1' \
    'r-irkernel=0.8*' \
    'r-plyr=1.8*' \
    'r-devtools=1.13*' \
    'r-tidyverse=1.2*' \
    'r-shiny=1.2*' \
    'r-rmarkdown=1.11*' \
    'r-forecast=8.2*' \
    'r-rsqlite=2.1*' \
    'r-reshape2=1.4*' \
    'r-nycflights13=1.0*' \
    'r-caret=6.0*' \
    'r-rcurl=1.95*' \
    'r-crayon=1.3*' \
    'r-randomforest=4.6*' \
    'r-htmltools=0.3*' \
    'r-sparklyr=0.9*' \
    'r-htmlwidgets=1.2*' \
    'r-hexbin=1.27*' && \
    conda remove --quiet --yes --force qt pyqt && \
    conda clean -tipsy && \
    # Activate ipywidgets extension in the environment that runs the notebook server
    jupyter nbextension enable --py widgetsnbextension --sys-prefix && \
    # Also activate ipywidgets extension for JupyterLab
    # Check this URL for most recent compatibilities
    # https://github.com/jupyter-widgets/ipywidgets/tree/master/packages/jupyterlab-manager
    jupyter labextension install @jupyter-widgets/jupyterlab-manager@^0.38.1 && \
    jupyter labextension install jupyterlab_bokeh@0.6.3 && \
    npm cache clean --force && \
    rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
    rm -rf /home/$NB_USER/.cache/yarn && \
    rm -rf /home/$NB_USER/.node-gyp && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# Install facets which does not have a pip or conda package at the moment
RUN cd /tmp && \
    git clone https://github.com/PAIR-code/facets.git && \
    cd facets && \
    jupyter nbextension install facets-dist/ --sys-prefix && \
    cd && \
    rm -rf /tmp/facets && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME /home/$NB_USER/.cache/
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions /home/$NB_USER

USER root

USER $NB_UID
RUN conda install -c conda-forge feather-format

# Bash kernel
RUN pip install bash_kernel
RUN python -m bash_kernel.install --user

# Octave kernel
RUN pip install octave_kernel
RUN python -m octave_kernel install --user

#Scala kernel
ENV SCALA_VERSION 2.11.12 
ENV ALMOND_VERSION 0.2.2

USER $NB_UID

RUN curl -L -o coursier https://git.io/coursier-cli 
RUN chmod +x coursier && \
    ./coursier --help
RUN ./coursier bootstrap \
    -r jitpack \
    -i user -I user:sh.almond:scala-kernel-api_$SCALA_VERSION:$ALMOND_VERSION \
    sh.almond:scala-kernel_$SCALA_VERSION:$ALMOND_VERSION \
    -o almond
RUN ./almond --install

# IJava kernel
USER root

RUN curl -L https://github.com/SpencerPark/IJava/releases/download/v1.2.0/ijava-1.2.0.zip > ijava-kernel.zip
RUN unzip ijava-kernel.zip -d ijava-kernel \
  && cd ijava-kernel \
  && python install.py --sys-prefix

USER $NB_UID

# JavaScript kernel
RUN npm rebuild
RUN npm install -g ijavascript itypescript
RUN ijsinstall --spec-path=full
RUN its --install=local

# C++ kernel and xtensor
RUN conda install xeus-cling xwidgets notebook xtensor xtl xframe -c conda-forge/label/gcc7
RUN jupyter labextension install @jupyter-widgets/jupyterlab-manager

#Install some R libraries
RUN conda install -y -c bioconda gnuplot
RUN R --slave -e 'install.packages("feather", repos="http://cran.stat.ucla.edu/")' 

# Install SoS and extensions
RUN conda install -y feather-format -c conda-forge
RUN pip install nbformat --upgrade
RUN pip install sos-notebook sos-r sos-bash sos-matlab sos-scala sos-javascript sos-python sos-xeus-cling sos-java --upgrade

RUN python -m sos_notebook.install
RUN jupyter labextension install jupyterlab-sos

#Install jupyterlab-git tracking extension
RUN jupyter labextension install @jupyterlab/git
RUN pip install jupyterlab-git
RUN jupyter serverextension enable --py jupyterlab_git

#Install nbdime
RUN pip install --upgrade nbdime && \
    jupyter lab build

# #Run Jupyter Lab on start as joyvan
USER $NB_UID

EXPOSE 8888
# ENTRYPOINT ["/usr/local/bin/mvn-entrypoint.sh"]
CMD jupyter lab --ip='0.0.0.0' --port=8888 --no-browser
