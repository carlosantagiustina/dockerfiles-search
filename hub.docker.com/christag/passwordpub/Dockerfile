# ./container/Dockerfile
FROM python:3.7-alpine
ADD ./ /code
WORKDIR /code
RUN pip install -r requirements
CMD ["python", "start_server.py"]
