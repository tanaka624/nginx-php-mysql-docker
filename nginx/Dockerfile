FROM nginx:stable-alpine
COPY conf.d/*.conf /etc/nginx/conf.d/

RUN apk upgrade -U -a
RUN apk add --update curl ca-certificates
RUN update-ca-certificates
RUN apk add --update tzdata
RUN cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
RUN apk del --purge tzdata
RUN rm -rf /var/cache/apk/*
