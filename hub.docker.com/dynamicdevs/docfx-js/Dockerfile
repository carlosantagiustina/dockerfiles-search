FROM dynamicdevs/mono-node:latest
ENV DOCFX_VER 2.47
# Installing DocFx
RUN npm install -g node2docfx \
    && apt-get update && apt-get install unzip wget git -y \
    && wget -q -P /tmp https://github.com/dotnet/docfx/releases/download/v${DOCFX_VER}/docfx.zip \
    && mkdir -p /opt/docfx \
    && unzip /tmp/docfx.zip -d /opt/docfx \
    && echo '#!/bin/bash\nmono /opt/docfx/docfx.exe $@' > /usr/bin/docfx \
    && chmod +x /usr/bin/docfx \
    && rm -f /tmp/*

# Installing nuget DocFx plugins
RUN nuget install memberpage -OutputDirectory /docfx/plugins && \
    nuget install rest.tagpage -OutputDirectory /docfx/plugins && \
    nuget install rest.operationpage -OutputDirectory /docfx/plugins

WORKDIR /docfx
CMD [ "/bin/bash", "-c", "/usr/lib/node_modules/node2docfx/node2docfx.js node2docfx.json && docfx build" ]