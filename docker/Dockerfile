# To build the image, run `docker build` command from the root of the
# repository:
#
#    docker build -f docker/Dockerfile .
#
# There is an optional PYTHON_VERSION build argument which sets the
# version of python to build against. For example:
#
#    docker build -f docker/Dockerfile --build-arg PYTHON_VERSION=3.10 .
#
# An optional LIBOLM_VERSION build argument which sets the
# version of libolm to build against. For example:
#
#    docker build -f docker/Dockerfile --build-arg LIBOLM_VERSION=3.2.10 .
#


##
## Creating a builder container
##
ARG PYTHON_VERSION=3.10
FROM docker.io/python:${PYTHON_VERSION}-alpine

##
## Build libolm for matrix-nio e2e support
##

# Install libolm build dependencies
ARG LIBOLM_VERSION=3.2.10
RUN apk add --no-cache \
    make \
    cmake \
    gcc \
    g++ \
    git \
    libffi-dev \
    yaml-dev \
    python3-dev

# Build libolm
#
# Also build the libolm python bindings and place them at /python-libs
# We will later copy contents from both of these folders to the runtime
# container
COPY docker/build_and_install_libolm.sh /scripts/
RUN /scripts/build_and_install_libolm.sh ${LIBOLM_VERSION} /python-libs

# Install Postgres dependencies
RUN apk add --no-cache \
    musl-dev \
    libpq \
    postgresql-dev

COPY my_project_name/requirements.txt /requirements.txt
RUN pip3 install -r /requirements.txt

VOLUME ["/data"]

# Start the bot
ENTRYPOINT ["python3","/my_project_name/main.py", "/data/config.yaml"]
