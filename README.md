# docker-letsencrypt

* Test generating a cert:
    ```
    docker-compose run --rm certbot \
    certonly --webroot \
    --register-unsafely-without-email --agree-tos \
    --webroot-path=/data/letsencrypt \
    --staging -d www.example.com
    ```
* Generate the real cert:
    ```
    docker-compose run --rm certbot \
    certonly --webroot \
    --email me@example.com \
    --agree-tos --no-eff-email \
    --webroot-path=/data/letsencrypt \
    -d www.example.com
    ```
* Renew the cert:
    ```
    docker-compose run --rm certbot \
    renew --webroot \
    --webroot-path=/data/letsencrypt
    ```
* Cron job to renew cert and reload nginx:
    ```
    0 4 * * * cd /home/deploy/docker-letsencrypt && /usr/local/bin/docker-compose -f docker-compose.yml -f env/prod/compose.yml run --rm certbot renew --webroot --webroot-path=/data/letsencrypt --quiet && /usr/local/bin/docker-compose -f docker-compose.yml -f env/prod/compose.yml kill -s HUP web
    ```
