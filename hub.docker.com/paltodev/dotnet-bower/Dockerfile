FROM microsoft/dotnet:1.0-sdk-projectjson

RUN useradd -m app
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash - && \
   apt-get install -y nodejs && \
   npm install -g bower && \
   apt-get clean
