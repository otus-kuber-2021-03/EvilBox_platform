FROM nginx:1.19.9-alpine

RUN apk --no-cache add shadow && \
    usermod -u 1001 nginx && \
    sed -i '9s|/usr/share/nginx/html|/app|' /etc/nginx/conf.d/default.conf && \
    sed -i '2s|80|8000|' /etc/nginx/conf.d/default.conf && \
    sed -i '/server_name  localhost;/d' /etc/nginx/conf.d/default.conf && \
    mkdir /app && \
    echo 'Hello World!' > /app/homework.html && \
    chown -R nginx: /app

WORKDIR /app
EXPOSE 8000
