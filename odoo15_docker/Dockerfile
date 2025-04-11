FROM ubuntu:22.04

LABEL maintainer="MrHieu84-nnhieu.htb@gmail.com"
ENV LANG=C.UTF-8
# Set the timezone to Asia/Ho_Chi_Minh (Saigon, Vietnam)
ENV TZ=Asia/Ho_Chi_Minh

# Set timezone
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime  \
    && echo $TZ > /etc/timezone

# Update the system and install necessary packages
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git \
    wget \
    unzip \
    openssh-server \
    python3-pip \
    build-essential \
    python3-dev \
    python3-venv \
    python3-wheel \
    libfreetype6-dev \
    libxml2-dev \
    libzip-dev \
    libldap2-dev \
    libsasl2-dev \
    python3-setuptools \
    node-less \
    libjpeg-dev \
    zlib1g-dev \
    libpq-dev \
    libxslt1-dev \
    libtiff5-dev \
    libjpeg8-dev \
    libopenjp2-7-dev \
    liblcms2-dev \
    libwebp-dev \
    libharfbuzz-dev \
    libfribidi-dev \
    libxcb1-dev \
    wkhtmltopdf \
    && rm -rf /var/lib/apt/lists/*

# Create user odoo15 with home directory and shell
RUN useradd -m -d /opt/odoo15 -U -r -s /bin/bash odoo15
USER odoo15

# Clone Odoo repository
RUN git clone https://www.github.com/odoo/odoo --depth 1 --branch 15.0 /opt/odoo15/odoo

# Make the requirements file readable by all users
RUN chmod a+r /opt/odoo15/odoo/requirements.txt


USER root

# Modify requirements.txt to change gevent version, fix when install on ubuntu version 24.2, on ubuntu 22.04 don't need
RUN sed -i 's/gevent==21.8.0/gevent==24.11.1/' /opt/odoo15/odoo/requirements.txt
RUN sed -i 's/greenlet==1.1.2/greenlet==3.1.1/' /opt/odoo15/odoo/requirements.txt

RUN pip3 install wheel
RUN pip3 install -r /opt/odoo15/odoo/requirements.txt 




USER odoo15

CMD ["/opt/odoo15/odoo/odoo-bin", "-c", "/etc/odoo15.conf"]



