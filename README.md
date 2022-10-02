# php-sweph

## Swiss Epehemeris license
Please pay attention and review license terms and conditions at the [official AstroDienst page](). I do not have a valid license, so no Swiss Ephemeris code/data files 
are redistributed here - just mere links to official distribution site.     

## Preface
Initially this was a project to port php-sweph to PHP7. I've somehow managed to craft the working code back in the 2017, but had 
little motivation to put the code open. Maybe now I'm rather late with this, but as proverb says _better late than never_.

## Build

### Preparation part
Spin up a fresh AlmaLinux8 VM with minimal s/w set, then enter commands as follows:

    dnf install -y epel-release 
    dnf install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm
    dnf module reset php
    dnf module install -y php:remi-7.4
    dnf install -y php74-php-devel php-devel git

Now your VM can build extensions for PHP and other C sources.

### Swiss Ephemeris part

OK, next one:

    mkdir /opt/sweph

Download `swe_unix_src_<version>.tar.gz` from [AstroDienst site](https://www.astro.com/ftp/swisseph/), then put to `/opt/sweph` dir of VM filesystem.

    tar xvfz swe_unix_src_<version>.tar.gz
    cd /opt/sweph/src
    make libswe.a libswe.so
    ln -s /opt/sweph/src /usr/src/sweph
    ln -s /opt/sweph/src/libswe.so /usr/lib64/libswe.so

Now you have a working copy of Swiss Ephemeris library.

### Extension part

    git clone https://github.com/tnt4brain/php-sweph.git
    cd php-sweph
    phpize
    ./configure
    make install

## Configure PHP to load the extension
    echo "extension=sweph" > /etc/php.d/40-sweph.ini
    php -i | grep sweph

## Usage note

You can add custom ephemerides search path with 'SE_EPHE_PATH' environment variable - I guess you have to add it to PHP environment.

## Sources
[GitHub repo - the code source I've updated to support PHP7 back in 2017](https://github.com/acarlton/php-sweph)

[Original GitHub repo which has evolved and improved since then, now with perfect PHP7 support](https://github.com/cyjoelchen/php-sweph)
