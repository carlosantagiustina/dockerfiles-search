# KaveToolbox 3.0-Beta on ubuntu 14
FROM kave/kavetoolbox:3.0-Beta.u14.node
MAINTAINER KAVE <kave@kpmg.com>
RUN apt-get update
# hack linux version detection for windows builds (docker inconsistent!)
RUN rm -f /opt/KaveToolbox/pro
RUN mv /opt/KaveToolbox/3.0-Beta /opt/KaveToolbox_tmp
RUN rm -rf /opt/KaveToolbox
RUN sed -i 's/return output$/return "Ubuntu14"\n/' /opt/KaveToolbox_tmp/config/kaveinstall.py 
RUN /opt/KaveToolbox_tmp/scripts/KaveInstall
RUN rm  -rf /opt/KaveToolbox_tmp
