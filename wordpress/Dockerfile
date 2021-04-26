FROM wordpress:5.7-php7.4-apache

ARG WORDPRESS_FTP_USER
ARG WORDPRESS_FTP_PASSWORD

RUN apt update \
    && apt install -y whois \
    && PW=$(echo ${WORDPRESS_FTP_PASSWORD} | mkpasswd -s -m md5) \
    && useradd -m -s /bin/bash -G www-data -p ${PW} ${WORDPRESS_FTP_USER} \
    && chown ${WORDPRESS_FTP_USER}:www-data /var/www/html \
    && chmod 2775 /var/www/html \
    && chmod -R g+w /var/www/html \
    && find /var/www/html -type d | xargs chmod g+s

RUN apt update \
    && apt install -y vsftpd \
    && mkdir -p /var/run/vsftpd/empty \
    && sed -i -e 's/^listen=NO$/listen=YES/g' \
        -e 's/^listen_ipv6=YES$/listen_ipv6=NO/g' \
        -e 's/^#write_enable=YES$/write_enable=YES/g' \
        -e 's/^#local_umask=022$/local_umask=022/g' \
        /etc/vsftpd.conf

RUN LINES=$(wc -l /usr/local/bin/docker-entrypoint.sh | cut -d ' ' -f 1) \
    && sed -i -e "${LINES}i /usr/sbin/vsftpd /etc/vsftpd.conf &\n" /usr/local/bin/docker-entrypoint.sh
