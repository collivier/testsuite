FROM ubuntu as base

ARG HELM_VERSION=v3.2.4
ARG FILENAME=helm-${HELM_VERSION}-linux-amd64.tar.gz
ARG KUBECTL=v1.18.4

RUN apt update && apt install -y wget curl git

RUN curl -L https://get.helm.sh/${FILENAME} | tar zxv -C /tmp
RUN mv /tmp/linux-amd64/helm /usr/local/bin/
RUN helm repo add stable https://kubernetes-charts.storage.googleapis.com

RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$KUBECTL/bin/linux/amd64/kubectl
RUN mv ./kubectl /usr/local/bin/kubectl
RUN chmod +x /usr/local/bin/kubectl

WORKDIR /test_workspace

from base as ci_test
# add binary AND ONLY the binary in this container so we can fail if binary is broken without the rest of the source
COPY cnf-conformance .
RUN ./cnf-conformance setup verbose
RUN wget https://raw.githubusercontent.com/cncf/cnf-conformance/master/example-cnfs/coredns/cnf-conformance.yml
RUN ./cnf-conformance ip_addresses cnf-config=./cnf-conformance.yml verbose