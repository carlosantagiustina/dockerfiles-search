# Copyright (C) 2015-2016 Intel Corporation
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License version 2 as
# published by the Free Software Foundation.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License along
# with this program; if not, write to the Free Software Foundation, Inc.,
# 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

FROM crops/yocto:ubuntu-16.04-base

USER root

ADD https://raw.githubusercontent.com/crops/extsdk-container/master/restrict_useradd.sh  \
        https://raw.githubusercontent.com/crops/extsdk-container/master/restrict_groupadd.sh \
        https://raw.githubusercontent.com/crops/extsdk-container/master/usersetup.py \
        /usr/bin/
COPY poky-entry.py poky-launch.sh /usr/bin/
COPY sudoers.usersetup /etc/

# We remove the user because we add a new one of our own.
# The usersetup user is solely for adding a new user that has the same uid,
# as the workspace. 70 is an arbitrary *low* unused uid on debian.
RUN userdel -r yoctouser && \
    groupadd -g 70 usersetup && \
    useradd -N -m -u 70 -g 70 usersetup && \
    chmod 755 /usr/bin/usersetup.py \
        /usr/bin/poky-entry.py \
        /usr/bin/poky-launch.sh \
        /usr/bin/restrict_groupadd.sh \
        /usr/bin/restrict_useradd.sh && \
    echo "#include /etc/sudoers.usersetup" >> /etc/sudoers

#Add dotnet core support
RUN \
    wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb && \
    dpkg -i packages-microsoft-prod.deb && \
    apt-get install -y apt-transport-https && \
    apt-get update && \
    apt-get install -y libicu-dev dotnet-runtime-2.1 dotnet-runtime-deps-2.1 aspnetcore-runtime-2.1 && \
    apt-get install -y dotnet-sdk-2.1

#Variscite DART-MX8M Yocto Morty Host Dependencies
RUN \
    apt-get install -y zlib1g-dev

#Debian Development
RUN \
    apt-get install -y debootstrap bc lzop bison flex libssl-dev kmod \
    qemu binfmt-support qemu-user-static && \
    update-binfmts --enable

#Misc. Creature Comforts
RUN apt-get install -y nano vim

#Support running 32bit binaries
RUN dpkg --add-architecture i386 && sudo apt-get update && \
  apt-get install -y libc6:i386 libncurses5:i386 libstdc++6:i386

#Support ./configure
RUN apt-get install -y automake autoconf libtool

USER usersetup
ENV LANG=en_US.UTF-8

ENTRYPOINT ["/usr/bin/dumb-init", "--", "/usr/bin/poky-entry.py"]
