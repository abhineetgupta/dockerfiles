# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.

# Modified by Abhineet Gupta

ARG BASE_CONTAINER=jupyter/minimal-notebook
FROM $BASE_CONTAINER

LABEL maintainer="Jupyter Project <jupyter@googlegroups.com>"

USER root

# ffmpeg for matplotlib anim & dvipng for latex labels
RUN apt-get update && \
    apt-get install -y --no-install-recommends ffmpeg dvipng tmux zsh && \
    rm -rf /var/lib/apt/lists/*

USER $NB_UID

# Install Python 3 packages
RUN conda install --quiet --yes \
    'beautifulsoup4=4.9.*' \
    'conda-forge::blas=*=openblas' \
    'bokeh=2.2.*' \
    'bottleneck=1.3.*' \
    'cloudpickle=1.6.*' \
    'cython=0.29.*' \
    'dask=2021.2.*' \
    'dill=0.3.*' \
    'h5py=3.1.*' \
    'ipywidgets=7.6.*' \
    'ipympl=0.6.*' \
    'matplotlib-base=3.3.*' \
    'numba=0.52.*' \
    'numexpr=2.7.*' \
# bug in pandas 1.2.* - https://github.com/pandas-dev/pandas/issues/39433
    'pandas=1.0.*' \
    'patsy=0.5.*' \
    'protobuf=3.14.*' \
    'pytables=3.6.*' \
    'scikit-image=0.18.*' \
    'scikit-learn=0.24.*' \
    'scipy=1.6.*' \
    'seaborn=0.11.*' \
    'sqlalchemy=1.3.*' \
    'statsmodels=0.12.*' \
    'sympy=1.7.*' \
    'vincent=0.4.*' \
    'widgetsnbextension=3.5.*'\
    'xlrd=2.0.*'  \
# additional packages not in default scipy notebook. Commented packages were not able to resolve dependencies
    'black=20.8b1' \
#    'hdf5=1.12.*' \
    'line_profiler=3.1.*' \
    'pytest=6.2.*' \
#    'unittest2=1.1.*' \
    && \
    conda clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Activate ipywidgets extension in the environment that runs the notebook server
#RUN jupyter nbextension enable --py widgetsnbextension --sys-prefix
#    # Also activate ipywidgets extension for JupyterLab
#    # Check this URL for most recent compatibilities
#    # https://github.com/jupyter-widgets/ipywidgets/tree/master/packages/jupyterlab-manager
#RUN jupyter labextension install @jupyter-widgets/jupyterlab-manager@^2.0.0 --no-build
#RUN jupyter labextension install @bokeh/jupyter_bokeh@^2.0.0 --no-build
#RUN jupyter labextension install jupyter-matplotlib@^0.7.2 --no-build
#RUN jupyter lab build -y --dev-build=False --minimize=False
#RUN jupyter lab clean -y
#RUN npm cache clean --force
#RUN rm -rf "/home/${NB_USER}/.cache/yarn"
#RUN rm -rf "/home/${NB_USER}/.node-gyp"
#RUN fix-permissions "${CONDA_DIR}"
#RUN fix-permissions "/home/${NB_USER}"

# Install pip based packages that are not available on conda default channels
RUN pip install --quiet --no-cache-dir \
    'pandarallel' \
# conda was not able to solve environment for installing unittest2
#    'pytest' \
    'unittest2' \
    && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"


# Install facets which does not have a pip or conda package at the moment
WORKDIR /tmp
RUN git clone https://github.com/PAIR-code/facets.git && \
    jupyter nbextension install facets/facets-dist/ --sys-prefix && \
    rm -rf /tmp/facets && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME="/home/${NB_USER}/.cache/"

RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions "/home/${NB_USER}"

USER $NB_UID

# Set up our notebook config.
COPY notebook.json /home/${NB_USER}/.jupyter/nbconfig/

WORKDIR $HOME

# install oh-my-zsh
RUN wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O - | zsh || true
COPY .zshrc /home/${NB_USER}/.zshrc

# docker build -t devenv .