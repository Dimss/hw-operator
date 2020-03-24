FROM registry.access.redhat.com/ubi7/ubi-minimal:latest

ENV OPERATOR=/usr/local/bin/hw-operator \
    USER_UID=1001 \
    USER_NAME=hw-operator

# install operator binary
COPY build/_output/bin/hw-operator ${OPERATOR}

COPY build/bin /usr/local/bin
RUN  /usr/local/bin/user_setup

RUN mvn package

RUN npm build

ENTRYPOINT ["/usr/local/bin/entrypoint"]

USER ${USER_UID}
