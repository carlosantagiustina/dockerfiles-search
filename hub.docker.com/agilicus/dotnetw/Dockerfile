FROM alpine as fetch
RUN apk add curl \
 && printf "\nfetch CR\n\n" \
 && curl -sL https://origin.softwaredownloads.sap.com/public/file/0020000000899062019 > /tmp/cr-32.zip \
 && printf "\nfetch wine-mono\n\n" \
 && curl -sL https://dl.winehq.org/wine/wine-mono/4.9.2/wine-mono-4.9.2.msi > /tmp/wine-mono-2.9.2.msi \
 && printf "\nfetch wine-gecko\n\n" \
 && curl -sL http://dl.winehq.org/wine/wine-gecko/2.47/wine_gecko-2.47-x86.msi > /tmp/wine_gecko-2.47-x86.msi

FROM mono:5 AS build
ADD ./fcgi /tmp/fcgi
WORKDIR /app
RUN apt-get update \
 && apt-get install -y git \
 && git clone https://github.com/Agilicus/xsp.git \
 && cd xsp \
 && git checkout 5b68b25db1be92243c8b9533f0a344b5562418a3 \
 && cd src/Mono.WebServer         && nuget restore -PackagesDirectory ../packages && cd ../.. \
 && cd src/Mono.WebServer.FastCgi && nuget restore -PackagesDirectory ../packages && cd ../.. \
 && cd src/Mono.WebServer         && cp AssemblyInfo4.cs.in AssemblyInfo4.cs                                 && msbuild /p:Configuration=Release && cd ../.. \
 && cd src/Mono.WebServer.FastCgi && sed -e 's?@XSP_VERSION@?0.4.0.0?g' AssemblyInfo.cs.in > AssemblyInfo.cs && msbuild /p:Configuration=Release && cd ../.. \
 && cp src/Mono.WebServer/bin/Release/* /tmp/fcgi \
 && cp src/Mono.WebServer.FastCgi/bin/Release/* /tmp/fcgi \
 && ls /tmp/fcgi

FROM ubuntu:18.04
LABEL maintainer="don@agilicus.com"

WORKDIR /app

# This scary-looking variable only causes apt-key to not warn on the
# output not being stdout (output should not be parsed).
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn
ENV DEBIAN_FRONTEND=noninteractive

RUN dpkg --add-architecture i386 \
 && apt-get update \
 && apt-get -y upgrade \
 && apt-get -y install curl gnupg2 dumb-init libfcgi0ldbl software-properties-common \
 && add-apt-repository -y ppa:nginx/stable \
 && apt-get -y install nginx-extras \
 && curl -L https://dl.winehq.org/wine-builds/winehq.key | apt-key add - \
 && echo "deb https://dl.winehq.org/wine-builds/ubuntu/ bionic main" >> /etc/apt/sources.list \
 && apt-get update \
 && curl -L https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/xUbuntu_18.04/amd64/libfaudio0_19.07-0~bionic_amd64.deb > /tmp/libfaudio0_19.07-0~bionic_amd64.deb \
 && curl -L https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/xUbuntu_18.04/i386/libfaudio0_19.07-0~bionic_i386.deb > /tmp/libfaudio0_19.07-0~bionic_i386.deb \
 && dpkg -i --force-all /tmp/libf*deb \
 && apt --fix-broken -y install \
 && echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | debconf-set-selections \
 && apt-get install -y --install-recommends winehq-stable wine-stable wine-stable-amd64 wine-stable-i386 xvfb x11vnc cabextract libntlm0 libwbclient0 ttf-mscorefonts-installer winbind unzip wget \
 && curl -sL https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks > /usr/local/bin/winetricks \
 && chmod a=rx /usr/local/bin/winetricks \
 && useradd --create-home -s /bin/bash dotnet \
 && mkdir -p /app \
 && chown dotnet:dotnet /app \
 && chown -R dotnet:dotnet /var/log/nginx /etc/nginx \
 && rm -rf /var/lib/apt/lists/* /tmp/* /home/dotnet/.wine/drive_c/users/dotnet/Temp/* \
 && mkdir -p /tmp/.X11-unix \
 && chmod 1777 /tmp/.X11-unix


USER dotnet
ENV WINEARCH=win32
ENV WINEDEBUG=-all

ENV WINEPATH='C:\\windows\\system32;C:\\windows;C:\\windows\\system32\\wbem;C:\\Program Files\\SAP BusinessObjects\\Crystal Reports for .NET Framework 4.0\\Common\\SAP BusinessObjects Enterprise XI 4.0\\win32_x86'
ENV DISPLAY=:1

COPY --chown=dotnet --from=fetch /tmp/* /tmp/
COPY --chown=dotnet --from=build /tmp/fcgi /tmp/fcgi
RUN Xvfb :1 & echo \
 && cd /tmp \
 && printf "\nCreate wine prefix\n\n" \
 && WINEDLLOVERRIDES="mscoree,mshtml=" wineboot -u \
 && printf "\nInstall wine-mono\n\n" \
 && wine msiexec /i /tmp/wine-mono-2.9.2.msi \
 && printf "\nInstall wine-gecko\n\n" \
 && wine msiexec /i wine_gecko-2.47-x86.msi \
 && printf "\nReboot\n\n" \
 && wineboot -s \
 && printf "\nInstall dotnet452\n\n" \
 && winetricks -q dotnet452 \
 && printf "\nInstall dotnet471\n\n" \
 && winetricks -q dotnet471 \
 && printf "\nInstall fonts\n\n" \
 && winetricks -q corefonts consolas cambria calibri arial courier georgia impact lucida tahoma trebuchet verdana webdings \
 && printf "\nunzip CR\n\n" \
 && unzip cr-32.zip \
 && printf "start CR phase(1)\n" \
 && wine msiexec /i CRforVS_redist_install_32bit_13_0_25/CRRuntime_32bit_13_0_25.msi /qn /norestart ALLUSERS=1 \
 && printf "Compile .net assemblies\n" \
 && (wine /home/dotnet/.wine/drive_c/windows/Microsoft.NET/Framework/v4.0.30319/ngen.exe update /nologo /force || /bin/true) \
 && printf "start CR phase(2)\n" \
 && wine msiexec /i CRforVS_redist_install_32bit_13_0_25/CRRuntime_32bit_13_0_25.msi /qn /norestart ALLUSERS=1 \
 && printf "done\n" \
 && sleep 1 \
 && (pkill Xvfb || /bin/true) \
 && cd /tmp/fcgi \
 && unzip gac.zip \
 && ls \
 && wine ./gacutil.exe /i fastcgi-mono-server4.exe \
 && wine ./gacutil.exe /i Mono.WebServer.dll \
 && wine ./gacutil.exe /i Mono.Posix.NETStandard.dll \
 && cd / \
 && rm -rf ~/.cache /tmp/.X1-lock /tmp/*

COPY fastcgi_params /etc/nginx/fastcgi_params
COPY nginx.conf /etc/nginx/nginx.conf
COPY Index.html /app
COPY entry-dotnetw.sh /

EXPOSE 5000
ENTRYPOINT [ "/usr/bin/dumb-init", "--" ]
CMD [ "/entry-dotnetw.sh" ]
