FROM python:2.7-alpine
RUN apk add --no-cache gcc musl-dev openssl-dev libffi-dev
COPY . /skeleton
WORKDIR /skeleton
RUN pip install -r requirements.txt
EXPOSE 5050
ENTRYPOINT ["ssh", "scripts/dev.sh"]
