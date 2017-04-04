FROM gcr.io/google_containers/hyperkube-amd64:v1.6.0
MAINTAINER Rostyslav Fridman <rostyslav_fridman@epam.com>

ADD testing.list /etc/apt/sources.list.d

# START: OpenvSwitch
RUN DEBIAN_FRONTEND=noninteractive apt-get update -y \
    && DEBIAN_FRONTEND=noninteractive apt-get upgrade -y \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
      openvswitch-common \
      openvswitch-switch \
      python-openvswitch \
      ovn-common \
      ovn-central \
      ovn-host \
    && DEBIAN_FRONTEND=noninteractive apt-get autoremove -y \
    && DEBIAN_FRONTEND=noninteractive apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
# END: OpenvSwitch

# START: OVN-K8S
RUN DEBIAN_FRONTEND=noninteractive apt-get update -y \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y python-setuptools \
    && easy_install pip \
    && pip install requests --upgrade \
    && git clone https://github.com/openvswitch/ovn-kubernetes.git \
    && cd ovn-kubernetes \
    && pip install . \
    && python setup.py build \
    && python setup.py install \
    && cd ../ \
    && rm -rf ovn-kubernetes \
    && easy_install -m pip \
    && ln -s /usr/local/bin/ovn-k8s-cni-overlay /opt/cni/bin/ovn_cni \
    && DEBIAN_FRONTEND=noninteractive apt-get autoremove -y \
    && DEBIAN_FRONTEND=noninteractive apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
# END: OVN-K8S
