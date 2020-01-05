FROM python:3-stretch

# Modified by Abhineet Gupta

# Pick up some dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        libfreetype6-dev \
        libhdf5-serial-dev \
        libpng-dev \
        libzmq3-dev \
        pkg-config \
        rsync \
        software-properties-common \
        unzip \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN pip3 --no-cache-dir install \
        Pillow \
        geopy \
        h5py \
        ipykernel \
        jupyter \
        matplotlib \
        numpy \
        pandas \
        psycopg2 \
        scipy \
        sklearn \
        scikit-image \
        sqlalchemy \
        && \
    python3 -m ipykernel.kernelspec

RUN ln -s -f /usr/bin/python3 /usr/bin/python

# Set up our notebook config.
COPY jupyter_notebook_config.py /root/.jupyter/

# Jupyter has issues with being run directly:
#   https://github.com/ipython/ipython/issues/7062
# We just add a little wrapper script.
COPY run_jupyter.sh /

# IPython
EXPOSE 8888

WORKDIR "/notebooks"

RUN chmod +x /run_jupyter.sh
CMD ["/run_jupyter.sh", "--allow-root"]

# docker build -t devenv .
