FROM nginx:alpine
MAINTAINER helbing <helbingxxx@gmail.com>

# 设置镜像源
ARG APK_REPOSITORY=false
ENV APK_REPOSITORY ${APK_REPOSITORY}

RUN if [ ${APK_REPOSITORY} != false ]; then \

    sed -i "1i$APK_REPOSITORY" /etc/apk/repositories \
;fi

RUN apk update && apk upgrade && \
    apk add --no-cache bash && \
    adduser -D -H -u 1000 -s /bin/bash www-data && \
    rm /etc/nginx/conf.d/default.conf

ADD nginx.conf /etc/nginx/

ARG PHP_ENABLE=false
ARG PHP_HOST=''

RUN if [ ${PHP_ENABLE} = true ]; then \
    echo "upstream php-upstream {" \
        "server ${PHP_HOST}:9000;" \
    "}" > /etc/nginx/conf.d/upstream.conf \
;fi

CMD ["nginx"]

EXPOSE 80 443
