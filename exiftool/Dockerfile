FROM perl:5.36-slim

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       curl 

RUN mkdir -p /opt/exiftool \
  && cd /opt/exiftool \
  && EXIFTOOL_VERSION=`curl -s https://exiftool.org/ver.txt` \
  && EXIFTOOL_ARCHIVE=Image-ExifTool-${EXIFTOOL_VERSION}.tar.gz \
  && curl -s -O https://exiftool.org/$EXIFTOOL_ARCHIVE \
  && CHECKSUM=`curl -s https://exiftool.org/checksums.txt | grep SHA1\(${EXIFTOOL_ARCHIVE} | awk -F'= ' '{print $2}'` \
  && tar xzf $EXIFTOOL_ARCHIVE --strip-components=1 \
  && rm -f $EXIFTOOL_ARCHIVE \
  && perl Makefile.PL \
  && make install

WORKDIR /tmp

ENTRYPOINT ["exiftool"]

CMD ["-ver"]


# to build - docker build -t exiftool .

# to run - docker run -it --rm -v "$PWD":/tmp exiftool -d %Y/%m "-directory<filemodifydate" "-directory<createdate" "-directory<datetimeoriginal" .