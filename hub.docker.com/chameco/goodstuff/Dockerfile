FROM haskell:8.4.3
RUN apt-get update
RUN apt-get install -y pkg-config libpcre3 libpcre3-dev postgresql postgresql-client libpq-dev sqlite3 curl
RUN curl -sL https://deb.nodesource.com/setup_11.x | bash -
RUN apt-get install -y nodejs
RUN npm install --global --unsafe-perm=true purescript pulp bower
COPY ./stack.yaml ./stack.yaml
COPY ./package.yaml ./package.yaml
RUN stack build --only-dependencies
COPY . .
RUN echo '{ "allow_root": true }' > /root/.bowerrc
RUN make
ENTRYPOINT ["./goodstuff"]
CMD ["web"]