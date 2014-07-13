<!-- 文字化け回避 -->
# nginx config

nginxの設定。ModSecurityを入れてない環境ではその行をエスケープ。

* 1CPU3CORE
* MEM2GB
* C10k　bench passed
* Wordpress
* phpMyAdmin
* Zabbix

```
/etc/nginx/nginx.conf
/etc/nginx/conf.d/protocol.http.conf
/etc/nginx/conf.d/server.web.conf
/etc/nginx/conf.d/web.guard.conf
/etc/nginx/conf.d/web.wordpress.conf
/etc/nginx/conf.d/web.phpMyAdmin.conf
/etc/nginx/conf.d/web.zabbix.conf
/etc/nginx/conf.d/web.static.conf

/etc/php-fpm.d/www.conf
/var/www/html/wp-config.php
```

php-fpm

```sh
sudo vi /etc/php-fpm.d/www.conf
listen = /var/run/php-fpm/php-fpm.sock
listen.owner = nginx
listen.group = nginx
 
user = nginx
group = nginx
 
pm = static
pm.max_children = 10
```

phpMyAdmin

```sh
sudo vi /etc/phpMyAdmin/config.inc.php
...
$cfg['blowfish_secret'] = '<keyword>';
...
$cfg['Servers'][$i]['auth_type']     = 'cookie';

sudo htpasswd -c /etc/nginx/htpasswd <username>
sudo ln -s /usr/share/phpMyAdmin /var/www/html/phpMyAdmin
```

Zabbix

```sh
sudo ln -s /usr/share/zabbix /var/www/html/zabbix
```

Wordpress

```sh
sudo vi /var/www/html/wp-config.php
// !!絶対にwp-settings.phpより先に設定すること!!
define('FORCE_SSL_LOGIN', true);
define('FORCE_SSL_ADMIN', true);
if ($_SERVER['HTTP_X_FORWARDED_PROTO'] == 'https')
        $_SERVER['HTTPS']='on';
 
 
 
/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');
```

## ChangeLog

### 0.0.1

* 公開
