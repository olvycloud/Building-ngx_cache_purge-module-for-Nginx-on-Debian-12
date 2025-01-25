# Building ngx_cache_purge module for Nginx on Debian-12

This guide provides step-by-step instructions to build `ngx_cache_purge` module for Nginx on Debian 12.
`ngx_cache_purge` module provides a robust and flexible solution for purging cache, even in environments with multi-user setups.

> **Note:** While Debian 12 ships with the `libnginx-mod-http-cache-purge package`, it does not support cache
>  purging if your PHP-FPM runs as a different user. This is because cache files are created with strict ownership
> (www-data) and permissions (600 for files, 700 for directories), preventing other users from modifying or purging them.

### 1. Enable Debian source repositories
```
deb http://deb.debian.org/debian bookworm main contrib non-free-firmware
deb-src http://deb.debian.org/debian bookworm main contrib non-free-firmware

deb http://deb.debian.org/debian bookworm-updates main contrib non-free-firmware
deb-src http://deb.debian.org/debian bookworm-updates main contrib non-free-firmware

deb http://security.debian.org/debian-security bookworm-security main contrib non-free-firmware
deb-src http://security.debian.org/debian-security bookworm-security main contrib non-free-firmware
```

### 2. Update packages sources
```
apt update
```

### 3. Install build dependencies
```
apt install dpkg-dev build-essential libpcre3-dev libssl-dev zlib1g-dev libxml2-dev libxslt1-dev libgd-dev libgeoip-dev libperl-dev
```

### 4. Download Nginx sources
```
apt source nginx
```

### 5. Download sources of `ngx_cache_purge` module
```
wget https://github.com/torden/ngx_cache_purge/archive/refs/tags/v2.3.1.tar.gz
```

### 7. Preparation step
Unpack `ngx_cache_purge` module:
```
tar xzf v2.3.1.tar.gz
```
Change to Nginx sources directory:
```
cd nginx-1.22.1
```

### 8. Configure and build `ngx_cache_purge` module
```
./configure --add-dynamic-module=../ngx_cache_purge-2.3.1 --with-cc-opt='-g -O2 -ffile-prefix-map=/build/nginx-nduIyd/nginx-1.22.1=. -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -fPIC' --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=stderr --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --modules-path=/usr/lib/nginx/modules --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-compat --with-debug --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_v2_module --with-http_dav_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_secure_link_module --with-http_sub_module --with-mail_ssl_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-stream_realip_module --with-http_geoip_module=dynamic --with-http_image_filter_module=dynamic --with-http_perl_module=dynamic --with-http_xslt_module=dynamic --with-mail=dynamic --with-stream=dynamic --with-stream_geoip_module=dynamic
```
```
make modules
```

If compilation process finished successfully, find `ngx_cache_purge` module in:
```
ls -l objs/ngx_http_cache_purge_module_custom.so
```

### 9. Install `ngx_cache_purge` module

Upload `ngx_http_cache_purge_module_custom.so` to your machine with Debian 12.

Include this module to your Nginx configuration:
```
load_module modules/ngx_http_cache_purge_module.so;
```

## About Olvy

Olvy is a private and independent Limited Liability Company based in Bratislava, Slovakia, offering innovative and reliable Managed Cloud Hosting services. 

For more information, visit [Olvy's website](https://olvy.net/).
