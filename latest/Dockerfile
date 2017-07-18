FROM jetbrains/teamcity-agent
LABEL maintainer "Jacek Kleczkowski <jacek@ksoft.biz>"

# image is based on Ubuntu 15 - fix for .NET Core installation
RUN apt-get install wget -y && \
    wget http://launchpadlibrarian.net/201330288/libicu52_52.1-8_amd64.deb && \
    dpkg -i libicu52_52.1-8_amd64.deb

# install .NET Core
RUN sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893 && \
    apt-get update && \
    apt-get install mc -y && \
    apt-get upgrade -y

# install .NET Core
ENV DOTNET_VER "dotnet-dev-2.0.0-preview1-005977"
#(apt-cache search dotnet-dev-2.0.0 | awk '{print $1}') | sh 
RUN apt-get install ${DOTNET_VER} -y

# install .net core sdk
RUN apt-get install dotnet-dev-1.0.4 -y 

# install mono-devel
RUN apt-get install mono-devel mono-xbuild libmono-addins-* -y 
    
# intall nodejs
RUN apt-get install nodejs -y && \
    apt-get install nodejs-legacy -y 
    
# gpg keys listed at https://github.com/nodejs/node
RUN set -ex \
  && for key in \
    9554F04D7259F04124DE6B476D5A82AC7E37093B \
    94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
    0034A06D9D9B0064CE8ADF6BF1747F4AD2306D93 \
    FD3A5288F042B6850C66B31F09FE44734EB7990E \
    71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
    DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
    B9AE9905FFD7803F25714661B63B535A4C206CA9 \
    C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
  ; do \
    gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
  done

ENV NODE_VERSION 6.11.1

RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz" \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --verify SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-x64.tar.gz\$" SHASUMS256.txt.asc | sha256sum -c - \
  && tar -xzf "node-v$NODE_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 \
  && rm "node-v$NODE_VERSION-linux-x64.tar.gz" SHASUMS256.txt.asc \
  && npm install npm -g \
  && npm install bower -g \
  && npm install gulp -g \
  && npm install webpack -g \
  && npm install karma -g \
  && npm install tslint -g \
  && npm install @angular/cli -g \ 
  && npm cache clear
        
VOLUME /opt/buildagent/work
VOLUME /opt/buildagent/logs
VOLUME /data/teamcity_agent/conf
VOLUME /opt/buildagent/plugins

ENV DOCKER_HOST ""
ENV DOCKER_BIN "/usr/bin/docker"



