#
# PX4 ROS2 development environment
# Based from container under https://github.com/osrf/docker_images/tree/master/ros2/source/devel
#

FROM px4io/px4-dev-ros-melodic:2019-12-18
LABEL maintainer="Nuno Marques <n.marques21@hotmail.com>"

# setup environment
ENV ROS1_DISTRO melodic
ENV ROS2_DISTRO eloquent

# setup ros2 keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

# setup sources.list
RUN echo "deb http://packages.ros.org/ros2/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros2-latest.list \
	&& apt-get update

# install ros2 desktop
RUN apt-get install -y --quiet \
		python3-colcon-common-extensions \
		python3-vcstool \
		ros-$ROS2_DISTRO-desktop \
		ros-$ROS2_DISTRO-launch-testing-ament-cmake \
		ros-$ROS2_DISTRO-rosidl-generator-dds-idl \
	&& apt-get -y autoremove \
	&& apt-get clean autoclean \
	&& rm -rf /var/lib/apt/lists/{apt,dpkg,cache,log} /tmp/* /var/tmp/*

# install python packages needed for testing
RUN curl https://bootstrap.pypa.io/get-pip.py | python3 \
	&& python3 -m pip install --upgrade pip \
		argcomplete \
		colcon-common-extensions \
		colcon-mixin \
		flake8 \
		flake8-blind-except \
		flake8-builtins \
		flake8-class-newline \
		flake8-comprehensions \
		flake8-deprecated \
		flake8-docstrings \
		flake8-import-order \
		flake8-quotes \
		pytest-repeat \
		pytest-rerunfailures

RUN rosdep update

# setup colcon mixin and metadata
RUN colcon mixin add default https://raw.githubusercontent.com/colcon/colcon-mixin-repository/master/index.yaml \
	&& colcon mixin update \
	&& colcon metadata add default https://raw.githubusercontent.com/colcon/colcon-metadata-repository/master/index.yaml \
	&& colcon metadata update

# Intall foonathan_memory from source as it is required to Fast-RTPS >= 1.9
RUN git clone https://github.com/eProsima/foonathan_memory_vendor.git /tmp/foonathan_memory \
	&& cd /tmp/foonathan_memory \
	&& mkdir build && cd build \
	&& cmake .. \
	&& cmake --build . --target install \
	&& rm -rf /tmp/*

# Install Fast-RTPS 1.9.2
RUN rm -rf /usr/local/include/fastrtps /usr/local/share/fastrtps /usr/local/lib/libfastrtps* /usr/local/bin/fastrtpsgen \
	&& git clone --recursive https://github.com/eProsima/Fast-RTPS.git -b 1.9.x /tmp/FastRTPS-1.9.2 \
	&& cd /tmp/FastRTPS-1.9.2 \
	&& mkdir build && cd build \
	&& cmake -DTHIRDPARTY=ON -DSECURITY=ON .. \
	&& cmake --build . --target install \
	&& rm -rf /tmp/*

# Fast-RTPS-Gen (required since Fast-RTPS-Gen got split from Fast-RTPS repo since 1.8.x)
RUN git clone --recursive https://github.com/eProsima/Fast-RTPS-Gen.git -b v1.0.2 /tmp/Fast-RTPS-Gen \
	&& cd /tmp/Fast-RTPS-Gen \
	&& gradle assemble \
	&& cp share/fastrtps/fastrtpsgen.jar /usr/local/share/fastrtps/ \
	&& cp scripts/fastrtpsgen /usr/local/bin/ \
	&& rm -rf /tmp/*
