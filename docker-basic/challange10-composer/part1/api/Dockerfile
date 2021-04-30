FROM  python:3.9.2-slim-buster
LABEL      maintainer="ILDC Labs <@ildc>"

WORKDIR    /app

COPY       requirements.txt /app/

RUN        pip install -r requirements.txt

COPY       *.py ./

ENTRYPOINT [ "python3" ,"./main.py"]
