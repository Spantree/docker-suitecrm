FROM php:5.6-apache

ENV DOWNLOAD_URL https://github.com/salesagility/SuiteCRM/archive/v7.7.4.zip
ENV DOWNLOAD_FILE v7.7.4.zip
ENV EXTRACT_FOLDER SuiteCRM-7.7.4
ENV WWW_FOLDER /var/www/html
ENV WWW_USER www-data
ENV WWW_GROUP www-data

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y libcurl4-gnutls-dev libpng-dev libssl-dev libc-client2007e-dev libkrb5-dev unzip cron re2c python tree wget && \
    docker-php-ext-configure imap --with-imap-ssl --with-kerberos && \
    docker-php-ext-install mysql curl gd zip mbstring imap && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /tmp

RUN wget ${DOWNLOAD_URL}
RUN unzip $DOWNLOAD_FILE
RUN rm $DOWNLOAD_FILE
RUN rm -rf ${WWW_FOLDER}/* 
RUN cp -R ${EXTRACT_FOLDER}/* ${WWW_FOLDER}/ 
RUN chown -R ${WWW_USER}:${WWW_GROUP} ${WWW_FOLDER}/* 
RUN chown -R ${WWW_USER}:${WWW_GROUP} ${WWW_FOLDER}

ADD php.ini /usr/local/etc/php/php.ini
ADD config_override.php.pyt /usr/local/src/config_override.php.pyt
ADD envtemplate.py /usr/local/bin/envtemplate.py
ADD init.sh /usr/local/bin/init.sh

RUN chmod u+x /usr/local/bin/init.sh
RUN chmod u+x /usr/local/bin/envtemplate.py

ADD crons.conf /root/crons.conf
RUN crontab /root/crons.conf

EXPOSE 80
ENTRYPOINT ["/usr/local/bin/init.sh"]

