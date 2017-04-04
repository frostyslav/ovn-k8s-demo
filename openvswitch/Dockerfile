FROM alpine:latest
MAINTAINER Rostyslav Fridman <rostyslav_fridman@epam.com>

RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
  && apk add --update --no-cache openvswitch openvswitch-dev openvswitch-bash-completion

RUN apk add --update --no-cache \
		libcap-ng-dev \
		ca-certificates \
		openssl \
		python \
		libatomic \
		supervisor

ADD configure-ovs.sh /usr/share/openvswitch/

# Create the database
RUN mkdir -p /usr/local/var/run/openvswitch/

# Install ovn-kubernetes
RUN apk add --update --no-cache --virtual .ovn-deps \
    py2-pip \
    python-dev \
    gcc \
    musl-dev \
    git \
  && git clone https://github.com/openvswitch/ovn-kubernetes \
  && cd ovn-kubernetes \
  && pip install . \
  && python setup.py build \
  && python setup.py install \
  && cd / \
  && rm -rf ovn-kubernetes \
  && apk del .ovn-deps

ADD supervisord.conf /etc/

RUN mkdir -p /var/log/supervisor/

CMD ["/usr/bin/supervisord"]
