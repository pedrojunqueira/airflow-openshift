FROM python:3.7

ARG AIRFLOW_VERSION=1.10.10

ENV AIRFLOW_HOME=/usr/local/airflow

ENV SLUGIFY_USES_TEXT_UNIDECODE=yes

ENV USER=airflow USER_ID=1001

# Install dependencies and tools
RUN apt-get update -yqq && \
    apt-get upgrade -yqq && \
    apt-get install -yqq --no-install-recommends \ 
    wget \
    libczmq-dev \
    curl \
    libssl-dev \
    git \
    inetutils-telnet \
    bind9utils freetds-dev \
    libkrb5-dev \
    libsasl2-dev \
    libffi-dev libpq-dev \
    freetds-bin build-essential \
    default-libmysqlclient-dev \
    apt-utils \
    rsync \
    zip \
    unzip \
    gcc \
    vim \
    locales \
    && apt-get clean

# Upgrade pip
# Install apache airflow with subpackages
RUN pip install --upgrade pip && \
    pip install apache-airflow[crypto,celery,postgres,hive,mysql,ssh,docker,hdfs,kubernetes,redis,rabbitmq,slack]==${AIRFLOW_VERSION} --constraint https://raw.githubusercontent.com/apache/airflow/1.10.10/requirements/requirements-python3.7.txt

RUN useradd -ms /bin/bash -d ${AIRFLOW_HOME} --uid ${USER_ID} airflow

COPY ./scripts/entrypoint.sh /entrypoint.sh

RUN chown -R airflow: ${AIRFLOW_HOME}

USER ${USER_ID}
WORKDIR ${AIRFLOW_HOME}
ENTRYPOINT ["/entrypoint.sh"]
