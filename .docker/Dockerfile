FROM debian:latest

MAINTAINER Adil YASSINE <ad.yassine@gmail.com>

RUN \
  apt update && \
  apt install -y --no-install-recommends wget ca-certificates nginx openssh-server && rm -rf /var/lib/apt/lists/*

RUN echo 'root:password' | chpasswd

RUN mkdir /var/run/sshd

RUN sed 's/PermitRootLogin without-password/PermitRootLogin yes/' -i /etc/ssh/sshd_config

RUN echo '<h1>Welcome to Expert DevOps Website</h1>' > /var/www/html/index.html

EXPOSE 80 22

CMD ["nginx", "-g", "daemon off;"]