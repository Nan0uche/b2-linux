FROM nginx

RUN apt-get update && apt-get upgrade -y

RUN adduser --system --uid 1000 --no-create-home --disabled-login --group nginxuser

RUN apt update -y

RUN chown -R nginxuser:nginxuser /etc/nginx

RUN chown -R nginxuser:nginxuser /var/cache/nginx

RUN touch /var/run/nginx.pid

RUN chown nginxuser:nginxuser /var/run/nginx.pid

USER nginxuser