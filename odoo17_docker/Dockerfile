FROM ubuntu:22.04

LABEL maintainer="MrHieu84-nnhieu.htb@gmail.com"


# Update and install required packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 python3-pip python3-dev python3-venv python3-wheel python3-setuptools \
    build-essential libxslt1-dev libzip-dev libldap2-dev libsasl2-dev libssl-dev \
    libpq-dev libjpeg-dev libpng-dev libwebp-dev zlib1g-dev libfreetype6-dev \
    libxml2-dev libtiff5-dev xfonts-75dpi xfonts-base xfonts-scalable \
    node-less npm curl git sudo wget unzip \
    && rm -rf /var/lib/apt/lists/*


# Create odoo17 user
RUN useradd -m -d /opt/odoo17 -U -r -s /bin/bash odoo17

# Switch to odoo17 user
USER odoo17

# Clone Odoo repository
RUN git clone https://www.github.com/odoo/odoo --depth 1 --branch 17.0 /opt/odoo17/odoo

# Make the requirements file readable by all users
RUN chmod a+r /opt/odoo17/odoo/requirements.txt

# Switch to root for installation
USER root
# Modify requirements.txt to change gevent version
RUN sed -i 's/gevent==21.8.0/gevent==24.11.1/' /opt/odoo17/odoo/requirements.txt
RUN sed -i 's/greenlet==1.1.2/greenlet==3.1.1/' /opt/odoo17/odoo/requirements.txt

# Install Python dependencies
RUN pip3 install --no-cache-dir wheel
RUN pip3 install --no-cache-dir -r /opt/odoo17/odoo/requirements.txt

# Switch back to odoo17 user
USER odoo17

# Start Odoo
CMD ["/opt/odoo17/odoo/odoo-bin", "-c", "/etc/odoo17.conf"]
