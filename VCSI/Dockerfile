From python:3.7-stretch

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        ffmpeg \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN pip3 install --upgrade pip && pip3 install \
		vcsi


RUN ln -s -f /usr/bin/python3 /usr/bin/python

COPY ./metadata_template.ini /videos/
WORKDIR "/videos"

# docker build -t vcsi .
# docker run  -it -v $(pwd):/videos/mnt --name vcsi --entrypoint /bin/bash vcsi
# vcsi "mnt/<filename>" -t -g 3x12 --end-delay-percent 2 --template metadata_template.ini -o mnt/output.png