FROM centos:centos7.9.2009

RUN rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 && \
    yum -y install \
    unzip \
    vim \
    httpd && \
    yum clean all && \
    localedef -f UTF-8 -i ja_JP ja_JP.UTF-8

RUN yum -y install http://rpms.famillecollet.com/enterprise/remi-release-7.rpm && \
    yum -y install --enablerepo=remi,remi-php73 php php-mbstring php-gd php-pdo php-mysqlnd php-pear php-devel php-xdebug && \
    yum clean all

# https://qiita.com/yatsbashy/items/02bbbebbfe7e5a5976bc
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/local.myhost

COPY ./Docker/php/my-host.conf /etc/httpd/conf.d/my-vhost.conf
COPY ./Docker/php/env.local /var/www/local.myhost/.env
COPY ./src/ /var/www/local.myhost/

RUN chmod -R 777 /var/www/local.myhost/storage && \
    composer install

EXPOSE 80

CMD ["/usr/sbin/apachectl","-DFOREGROUND"]
