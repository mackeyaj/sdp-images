# Copyright © 2022 Booz Allen Hamilton. All Rights Reserved.
# This software package is licensed under the Booz Allen Public License. The license can be found in the License file or at http://boozallen.github.io/licenses/bapl

ARG BASE_REGISTRY=registry.access.redhat.com
ARG BASE_IMAGE=ubi8/ubi
ARG BASE_TAG=8.4
FROM ${BASE_REGISTRY}/${BASE_IMAGE}:${BASE_TAG}

### Required Atomic/OpenShift Labels - https://github.com/projectatomic/ContainerApplicationGenericLabels
LABEL name="Solutions Delivery Platform: Newman" \
      maintainer="uip@bah.com" \
      vendor="Booz Allen Hamilton" \
      version="5.0.0" \
      release="5.0.0" \
      summary="Newman CLI container" \
      description="The Newman container image for the Solutions Delivery Platform"

### add licenses to this directory
COPY LICENSE /licenses

### Use yum as root user
USER root

### Install NPM repo
RUN curl -fsSL https://rpm.nodesource.com/setup_17.x | bash -

### Install nodejs
RUN yum clean all && \
  yum install -y nodejs

### NPM Install Newman and newman-reporter-htmlextra
RUN npm install -g newman && \
npm install -g newman-reporter-htmlextra

ENV PROGRAM_NAME newman
ENV PROGRAM_VERSION v5.0.0
ENV HOME /root

ARG user=${PROGRAM_NAME}

WORKDIR /scanner

# Add user, create required directories and cleanup
RUN useradd -ms /bin/bash ${user}                                                                              && \
  ##chown -R ${user}:${user} ${PROGRAM_DESTINATION_DIR}/${PROGRAM_NAME}                                         && \
  ##mkdir /report                                                                                              && \
  ##chown -R ${user}:${user} /report
  yum install -y git                                                                           && \
  yum --nogpgcheck --disableplugin=subscription-manager clean all

### Insert container entrypoint script
COPY container-entrypoint.sh /usr/local/bin/container-entrypoint.sh

### Set script as executable
RUN chmod +x /usr/local/bin/container-entrypoint.sh

USER ${user}

CMD []
ENTRYPOINT [ "/bin/sh", "/usr/local/bin/container-entrypoint.sh" ]
