FROM python:3-alpine

RUN pip install --no-cache-dir requests pytz
COPY *.py ./

ENTRYPOINT ["python", "register.py"]
