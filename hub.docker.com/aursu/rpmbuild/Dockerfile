FROM aursu/rpmbuild:7-base-systemd

# install RPM build tools and assets
RUN yum -y install \
        deltarpm \
        rpm-build \
        yum-utils \
        epel-rpm-macros \
    && yum clean all && rm -rf /var/cache/yum /var/lib/rpm/__db*

COPY system/etc/yum.repos.d/custom.repo /etc/yum.repos.d/custom.repo
COPY system/etc/yum.repos.d/bintray-custom.repo /etc/yum.repos.d/bintray-custom.repo

# install common build tools (autotools, compilers, parsers etc)
RUN yum -y install \
        autoconf \
        bison \
        flex \
        gcc \
        gcc-c++ \
        libtool \
        make \
        which \
    && yum clean all && rm -rf /var/cache/yum /var/lib/rpm/__db*

VOLUME ${BUILD_TOPDIR}/SRPMS ${BUILD_TOPDIR}/RPMS
