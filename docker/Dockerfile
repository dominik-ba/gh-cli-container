FROM alpine:3.15.0

ENV LANG=C.UTF-8

# For COPY PASTE
# RUN true \
#     && xyz \
#     && true

# Necesserary build and run dependencies
ENV BUILD_DEPS='tar gzip curl'
RUN true \
    && apk --no-cache add $BUILD_DEPS \
    && apk add --update -t deps \
    && true

ENV RUN_DEPS='git'
RUN true \
    && apk add --update -t deps \
    && apk --no-cache add $RUN_DEPS \
    && true


RUN true \
    && mkdir /.config \
    && chmod 777 /.config \
    && true

ARG GH_CLI_VERSION_EXTERNAL
ENV GH_CLI_VERSION "2.6.0"
ENV GH_CLI_CHECKSUM 4ed92c712bfaa002c9421cee12cdf4cb0ae76fcd49d60cab5d316b8ce9c0314a
ENV GH_CLI_BASE_URL "https://github.com/cli/cli/releases/download"
ENV GH_CLI_FILE_NAME "gh_${GH_CLI_VERSION}_linux_amd64.tar.gz"
ENV GH_CLI_UNZIPPED_FILE_NAME "gh_${GH_CLI_VERSION}_linux_amd64"


# Two spaces between checksum and filename is needed and therefore on purpose
RUN true \
    && echo Downloading "${GH_CLI_BASE_URL}/v${GH_CLI_VERSION}/${GH_CLI_FILE_NAME}" \
    && curl -fOL "${GH_CLI_BASE_URL}/v${GH_CLI_VERSION}/${GH_CLI_FILE_NAME}" \
    && echo "${GH_CLI_CHECKSUM}  ${GH_CLI_FILE_NAME}" | sha256sum -c - \
    && tar -xzf ${GH_CLI_FILE_NAME} \
    && mv ./${GH_CLI_UNZIPPED_FILE_NAME}/bin/gh /usr/bin/gh \
    && chmod 0755 /usr/bin/gh \
    && rm ${GH_CLI_FILE_NAME} \
    && rm ${GH_CLI_UNZIPPED_FILE_NAME} -r \
    && true


# ARGUMENTS for non-root execution of a container
# where user id and group id is fix.
ARG CUSTOM_UID=1234
ARG CUSTOM_GID=1234

# add user & group for non-root usage
# contains usermod & groupmod
RUN apk --no-cache add shadow
RUN true \
    && groupadd -g $CUSTOM_GID custom_user \
    && useradd -u $CUSTOM_UID -d "/home" -g $CUSTOM_GID custom_user \
    && chown -vR $CUSTOM_UID:$CUSTOM_GID /home \
    && true
RUN mkdir /home/work
RUN chmod 777 /home/work
USER custom_user

WORKDIR /home/work

ENV GH_ENTERPRISE_TOKEN null
ENV GH_HOST null
ENV GH_REPO null

ENV CLICOLOR_FORCE 1
ENV CLICOLOR 1

ENTRYPOINT ["gh"]
