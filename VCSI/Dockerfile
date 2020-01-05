From python:3.7-stretch

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        ffmpeg \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN pip3 install \
		vcsi


RUN ln -s -f /usr/bin/python3 /usr/bin/python

WORKDIR "/videos"

# docker build -t vcsi .