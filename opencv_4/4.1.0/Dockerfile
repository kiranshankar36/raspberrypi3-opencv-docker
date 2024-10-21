FROM resin/raspberrypi3-python:2.7

# Install Debian repositories
RUN printf "deb http://archive.debian.org/debian/ jessie main\ndeb-src http://archive.debian.org/debian/ jessie main\ndeb http://security.debian.org jessie/updates main\ndeb-src http://security.debian.org jessie/updates main" > /etc/apt/sources.list

# Install dependencies needed for building and running OpenCV
RUN apt-get update && apt-get install -y --no-install-recommends \
    unzip \
    build-essential cmake pkg-config \
    checkinstall yasm \
    libjpeg-dev libtiff-dev libjasper-dev libpng12-dev libtiff5-dev \
    libavcodec-dev libavformat-dev libswscale-dev \
    libxine2-dev libv4l-dev

# Create symlink for compatibility
RUN ln -s -f /usr/include/libv4l1-videodev.h /usr/include/linux/videodev.h

RUN apt-get install -y --no-install-recommends \
    libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev \
    # needed by highgui tool
    libgtk2.0-dev \
    # for opencv math operations
    libatlas-base-dev gfortran \
    # others
    libtbb2 libtbb-dev qt5-default \
    libmp3lame-dev libtheora-dev \
    libvorbis-dev libxvidcore-dev libx264-dev \
    libopencore-amrnb-dev libopencore-amrwb-dev \
    libavresample-dev \
    x264 v4l-utils \
    # cleanup
    && rm -rf /var/lib/apt/lists/* \
    && apt-get -y autoremove

# Install Python packages
RUN pip install --no-cache-dir \
    # OpenCV dependency
    numpy \
    # other useful stuff
    ipython \
    # cleanup
    && find /usr/local \
       \( -type d -a -name test -o -name tests \) \
       -o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
       -exec rm -rf '{}' + \
    && rm -rf /usr/src/python ~/.cache

# Install OpenCV
COPY download_build_install_opencv.sh download_build_install_opencv.sh
RUN chmod +x download_build_install_opencv.sh
RUN ./download_build_install_opencv.sh
