# Dockerfile

# Use the official Ubuntu 22.04 base image
FROM ubuntu:22.04

# Environment
ARG SERVER_DOCKER_GITUSER=default_value

ARG SERVER_DOCKER_BITCOIN_VERSION=default_value

# Info
RUN bash -c "echo starting using github user SERVER_DOCKER_GITUSER = '${SERVER_DOCKER_GITUSER}' and SERVER_DOCKER_BITCOIN_VERSION = '${SERVER_DOCKER_BITCOIN_VERSION}' "

# Install apt-utils
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y apt-utils && \
    rm -rf /var/lib/apt/lists/*

# Install necessary build tools for C++
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y openjdk-19-jdk && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y git gradle curl wget openssl net-tools iputils-ping traceroute autoconf pkg-config libtool emacs && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential && \
    rm -rf /var/lib/apt/lists/*

# Install ssh server
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y openssh-server && \
    rm -rf /var/lib/apt/lists/*

# Install ssh client
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y openssh-client && \
    rm -rf /var/lib/apt/lists/*

# Colors
RUN echo '' >> ~/.bashrc
RUN echo 'LS_COLORS="di=1;93"' >> ~/.bashrc

# Install tomcat
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y tomcat9 && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /usr/local/src

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y automake autotools-dev bsdmainutils build-essential ccache clang gcc git libboost-dev libboost-filesystem-dev \
    libboost-system-dev libboost-test-dev libevent-dev libminiupnpc-dev libnatpmp-dev libsqlite3-dev libtool libzmq3-dev pkg-config python3 qtbase5-dev qttools5-dev \
    qttools5-dev-tools qtwayland5 systemtap-sdt-dev && \
    rm -rf /var/lib/apt/lists/*

ARG CACHEBUST

RUN echo "CACHEBUST is '$CACHEBUST'"

RUN git clone https://github.com/${SERVER_DOCKER_GITUSER}/blockchain-bitcoin.git

WORKDIR /usr/local/src/blockchain-bitcoin/bin

RUN ls -l

RUN chmod 700 ./c

RUN ./c -GITUSER "${SERVER_DOCKER_GITUSER}" -BITCOIN_VERSION "${SERVER_DOCKER_BITCOIN_VERSION}"

# Set the working directory
# WORKDIR /usr/local/src

# Expose the default Tomcat port
ENV CATALINA_BASE /var/lib/tomcat9
ENV CATALINA_HOME /usr/share/tomcat9
ENV PATH $CATALINA_HOME/bin:$PATH
EXPOSE 8080

# Create data dir for volume data
RUN mkdir /data 2>/dev/null

# Expose the default ssh port
EXPOSE 22

# # Start when the container starts
ENTRYPOINT ( service ssh start ) && ( catalina.sh run ) && ( tail -f /dev/null )
