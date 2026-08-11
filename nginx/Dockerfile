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

RUN rm -f /etc/nginx/http.d/default.conf

# Единый конфиг: серверы + сниппет «локального модуля»
COPY ./nginx/servers.conf /etc/nginx/http.d/servers.conf
COPY ./nginx/module-locations.conf /etc/nginx/module-locations.conf
COPY ./build /usr/share/nginx/html

EXPOSE 443
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
