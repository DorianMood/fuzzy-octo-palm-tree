FROM alpine:3.20

# Устанавливаем nginx с поддержкой Brotli + SSL CA
RUN apk add --no-cache nginx nginx-mod-http-brotli ca-certificates && \
    update-ca-certificates && \
    mkdir -p /run/nginx


RUN apk add --no-cache openssl
RUN mkdir -p /etc/nginx/ssl \
 && openssl req -x509 -nodes -days 365 \
    -newkey rsa:2048 \
    -keyout /etc/nginx/ssl/nginx.key \
    -out /etc/nginx/ssl/nginx.crt \
    -subj "/CN=localhost"

COPY ./nginx/no-compression.conf /etc/nginx/http.d/no-compression.conf
COPY ./nginx/gzip.conf /etc/nginx/http.d/gzip.conf
COPY ./nginx/brotli.conf /etc/nginx/http.d/brotli.conf
COPY ./nginx/https.conf /etc/nginx/http.d/https.conf
COPY ./build /usr/share/nginx/html

EXPOSE 443
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
