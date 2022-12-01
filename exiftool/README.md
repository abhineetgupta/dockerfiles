# exiftool
Docker setup for exiftool. Grabs the latest version of exiftool when building the image.

1. Build the docker image - 
    ```
    docker build -t exiftool .
    ```
2. The image's entrypoint is exiftool, so when running the container, any command following [exiftool](https://exiftool.org) may be used. As an example, the following command moves all images and videos in the current directory to folders partitioned by year and then month.
    ```
    docker run -it --rm -v "$PWD":/tmp exiftool -d %Y/%m "-directory<filemodifydate" "-directory<createdate" "-directory<datetimeoriginal" /tmp
    ```