# vim:set ft=dockerfile:
FROM postgres:9.6
MAINTAINER Erik van Weert <erik.van.weert@achmea.nl>

ENV TERM vt220

RUN set -eux; \
    gpg --no-tty --keyserver hkp://keyserver.ubuntu.com --recv-keys "C2518248EEA14886" && \
    gpg --batch --export "C2518248EEA14886" > /etc/apt/trusted.gpg.d/webupd8team.gpg && \
    echo deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main > /etc/apt/sources.list.d/webupd8team-java.list && \
    apt-get update && \
    apt-get clean && apt-get update && apt-get --fix-missing -y   --no-install-recommends install git ca-certificates apt-utils && \
    git clone https://github.com/tada/pljava.git && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
    apt-get clean && apt-get update && apt-get --fix-missing -y   --no-install-recommends install g++ maven && \
    apt-get clean && apt-get update && apt-get --fix-missing -y   --no-install-recommends install postgresql-server-dev-9.6 libpq-dev && \
    apt-get clean && apt-get update && apt-get --fix-missing -y   --no-install-recommends install libecpg-dev libkrb5-dev && \
    apt-get clean && apt-get update && apt-get --fix-missing -y   --no-install-recommends install oracle-java8-installer libssl-dev && \
    export PGXS=/usr/lib/postgresql/9.6/lib/pgxs/src/makefiles/pgxs.mk && \
    cd pljava && \
    git checkout REL1_5_STABLE && \
    mvn -X -Pwnosign clean install && \
    java -jar /pljava/pljava-packaging/target/pljava-pg9.6-amd64-Linux-gpp.jar && \
    cd ../ && \
    apt-get -y remove --purge --auto-remove git ca-certificates g++ maven postgresql-server-dev-9.6 libpq-dev libecpg-dev libkrb5-dev oracle-java8-installer libssl-dev && \
    apt-get clean && apt-get update && apt-get --fix-missing -y   --no-install-recommends install openjdk-8-jdk-headless && \
    apt-get -y clean autoclean autoremove && \
    rm -rf ~/.m2 /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY docker-entrypoint.sh /usr/local/bin/
#RUN ln -s usr/local/bin/docker-entrypoint.sh / # backwards compat
ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 5432
CMD ["postgres"]
