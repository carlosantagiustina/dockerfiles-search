# Build as jupyterhub/systemuser
# Run with the DockerSpawner in JupyterHub

FROM zonca/jupyterhub-datascience-singleuser

MAINTAINER Project Jupyter <jupyter@googlegroups.com>

USER root
RUN userdel jovyan
ENV SHELL /bin/bash
ADD systemuser.sh /srv/singleuser/systemuser.sh
# smoke test entrypoint
RUN USER_ID=65000 USER=systemusertest sh /srv/singleuser/systemuser.sh -h && userdel systemusertest

CMD ["sh", "/srv/singleuser/systemuser.sh"]
