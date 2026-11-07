FROM ubuntu:22.04

RUN apt-get update && apt-get install -y nginx curl

ENV ADMIN_EMAIL="envtest@email.com"

COPY index.html /var/www/html/index.html

RUN sed -i "s/MY_EMAIL/$ADMIN_EMAIL/g" /var/www/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]