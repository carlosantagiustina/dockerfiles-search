# Copyright (C) 2018  Christian Berger
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

# Part to build example.
FROM resin/armv7hf-debian:buster as builder
RUN [ "cross-build-start" ]
RUN apt-get update -y && apt-get install -y apt-utils
RUN apt-get install -y cmake g++ libopencv-dev libopencv-core-dev libopencv-highgui-dev make 

ADD . /opt/sources
WORKDIR /opt/sources
RUN mkdir build && \
    cd build && \
    cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/tmp .. && \
    make && make install 
RUN [ "cross-build-end" ]  


# Part to deploy example.
FROM resin/raspberry-pi-debian:buster
RUN [ "cross-build-start" ]
RUN apt-get -y update 
RUN apt-get -y install libopencv-calib3d3.2 libopencv-contrib3.2 libopencv-core3.2 libopencv-features2d3.2 libopencv-flann3.2 libopencv-highgui3.2 libopencv-imgcodecs3.2 libopencv-imgproc3.2 libopencv-ml3.2 libopencv-objdetect3.2 libopencv-photo3.2 libopencv-shape3.2 libopencv-stitching3.2 libopencv-superres3.2 libopencv-video3.2 libopencv-videoio3.2 libopencv-videostab3.2 libopencv-viz3.2

WORKDIR /usr/bin
COPY --from=builder /tmp/bin/example .

RUN [ "cross-build-end" ]  
