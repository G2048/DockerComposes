FROM ubuntu:20.04
MAINTAINER FW_IX <borntokill.ix@gmail.com>

ENV TZ="Europe/Moscow"
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

RUN apt update -y && apt upgrade -y
RUN ["apt", "install", "wget", "lsb-release", "gnupg2", "iproute2", "sudo", "systemd", "curl", "locales", "vim", "-y" ]

RUN sudo sh -c 'echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
RUN sudo apt-get update && sudo apt-get -y install postgresql
RUN service postgresql start

RUN wget http://distr.staffcop.su/install.sh -O /tmp/install.sh && bash /tmp/install.sh
RUN perl -i -nsplE 's/mount -t tmpfs/#mount -t tmpfs/' /usr/share/staffcop/staffcop
RUN ["staffcop", "init"]

VOLUME ["/var/lib/postgresql"]

RUN chown -R postgres:postgres /var/lib/postgresql/* && chmod -R 750 /var/lib/postgresql/*
RUN echo "host all  all  0.0.0.0/0 trust" >> /etc/postgresql/16/main/pg_hba.conf
#RUN echo "listen_addresses='*'" >> /etc/postgresql/16/main/postgresql.conf
RUN rm /etc/nginx/sites-enabled/default && service nginx restart
#RUN echo "ALTER USER staffcop WITH PASSWORD '$(perl -lnE "say \$2 if /('password':)\s'(.+)'/i" /etc/staffcop/config)';" | sudo -u postgres psql

CMD service postgresql start && staffcop dev

EXPOSE 80/tcp
EXPOSE 443/tcp

