FROM python:3.11.3-bullseye as builder

WORKDIR /usr/src/Farm

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN pip install --upgrade pip
RUN pip install flake8
COPY . . 
RUN flake8 --ignore=E501,F401 /usr/src/Farm/Farm

COPY ./requirement.txt .
RUN pip wheel --no-cache-dir --no-deps --wheel-dir /usr/src/Farm/wheels -r requirement.txt


FROM python:3.11.3-bullseye

RUN mkdir -p /home/Farm



ENV HOME=/home/Farm
ENV FARM_HOME=/home/Farm/web

RUN mkdir $FARM_HOME
RUN mkdir $FARM_HOME/static
RUN mkdir $FARM_HOME/media
WORKDIR $FARM_HOME



COPY --from=builder /usr/src/Farm/wheels /wheels
COPY --from=builder /usr/src/Farm/requirement.txt .
RUN pip install --no-cache /wheels/*

COPY . $FARM_HOME
RUN python manage.py migrate
RUN python manage.py collectstatic




