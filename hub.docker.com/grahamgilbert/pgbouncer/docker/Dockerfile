FROM jenkins/jnlp-slave:3.29-1

USER 0

RUN export FLYWAY_PATH=/flyway \
&&  export FLYWAY_VERSION=5.2.4 \
&&  export FLYWAY_REPO=https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline \
&&  export FLYWAY_FILE=flyway-commandline-${FLYWAY_VERSION}.tar.gz \
&&  export FLYWAY_URL=$FLYWAY_REPO/$FLYWAY_VERSION/$FLYWAY_FILE \
&&  mkdir $FLYWAY_PATH \
&&  curl -k -L $FLYWAY_URL | tar xz --strip-components=1 -C $FLYWAY_PATH \
&&  chmod -R g=u $FLYWAY_PATH \
&&  chmod -R o=u $FLYWAY_PATH

RUN export KRB5CCNAME=FILE:/tmp/tgt \
&& export DEBIAN_FRONTEND=noninteractive \
&& apt-get update \
&& apt-get install -y python-pip krb5-user libkrb5-dev \
&& pip install "ansible>=2.8" "docker" "requests" "openshift" "pywinrm[kerberos]" "kerberos" "dnspython" \
&& apt-get clean all

RUN export OC_PATH=/openshit \
&& export OC_VERSION=v3.11.0 \
&& export OC_REVISION=0cbc58b \
&& export OC_FILE=openshift-origin-client-tools-$OC_VERSION-$OC_REVISION-linux-64bit \
&& export OC_TAR=$OC_FILE.tar.gz \
&& export OC_REPO=https://github.com/openshift/origin/releases/download \
&& export OC_URL=$OC_REPO/$OC_VERSION/$OC_TAR \
&& curl -L $OC_URL | tar xvz -C /tmp \
&& mv /tmp/${OC_FILE} $OC_PATH \
&& chmod -R g=u $OC_PATH \
&& chmod -R o=u $OC_PATH

ENV PATH=/openshit:/flyway:$PATH

