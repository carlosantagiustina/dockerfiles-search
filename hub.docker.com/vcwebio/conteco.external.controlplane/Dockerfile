##### BEGIN external-extended/Dockerfile/iamge-header #####
FROM vcwebio/conteco.external.bash:5.0.9 AS base
##### END external-extended/Dockerfile/image-header #####

##### BEGIN image/Dockerfile/build-instructions #####
RUN apk update && apk add docker && apk add git && apk add inotify-tools && apk add nano && rm -rf /var/cache/apk/*
##### END image/Dockerfile/build-instructions #####

##### BEGIN external/Dockerfile/conteco #####
COPY ./ /repo/git-repository/
##### END external/Dockerfile/conteco #####

##### BEGIN external-mapped/Dockerfile/labels-footer #####
LABEL io.vcweb.schema-version="1.0" \
      io.vcweb.realm="vcwebio" \
      io.vcweb.ecosystem="conteco" \
      io.vcweb.type="external" \
      io.vcweb.name="controlplane" \
      io.vcweb.tag="18.09.8" \
      io.vcweb.base="external.bash:5.0.9" \
      io.vcweb.build="external-extended" \
      io.vcweb.label="external.controlplane - ContEco" \
      io.vcweb.description=" image with controlplane prerequisites installed." \
      io.vcweb.repository="/conteco/git-repository" 
##### END external-mapped/Dockerfile/labels-footer #####
