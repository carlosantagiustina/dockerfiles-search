FROM oznu/homebridge:latest

WORKDIR /defaults

# milight setup
#https://github.com/dotsam/homebridge-milight
RUN yarn add homebridge-milight

# yarn add homebridge-platform-wemo
RUN yarn add homebridge-platform-wemo

# mi-ir-remote setup
#https://github.com/Zzm317/homebridge-mi-ir-remote
RUN yarn add miio homebridge-mi-ir-remote
RUN yarn global add miio

# https://github.com/YinHangCode/homebridge-mi-outlet
RUN yarn add homebridge-mi-outlet

# homebridge-camera-ffmpeg setup
#https://github.com/KhaosT/homebridge-camera-ffmpeg
RUN apk add --no-cache ffmpeg
RUN yarn add homebridge-camera-ffmpeg

# homebridge-yeelight setup (not working yet)
RUN yarn add homebridge-yeelight

# homebridge-videodoorbell setup
#https://github.com/Samfox2/homebridge-videodoorbell
RUN yarn add homebridge-videodoorbell

# Alexa version of homebridge
# https://github.com/oznu/docker-homebridge/issues/30
RUN yarn global add https://github.com/NorthernMan54/homebridge

# homebridge-mi-aqara setup
#https://github.com/YinHangCode/homebridge-mi-aqara
RUN yarn add homebridge-mi-aqara

COPY defaults/startup.sh /defaults/startup.sh

WORKDIR /homebridge