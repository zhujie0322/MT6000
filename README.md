# Actions-OpenWrt

Build OpenWrt using GitHub Actions

[原作者的中文教程](https://p3terx.com/archives/build-openwrt-with-github-actions.html)

## Usage

### 自用GL.iNet MT6000固件
基于[Lede](https://github.com/coolsnowwolf/lede)大神固件编译
|IP|用户名|密码|
|:--:|:--:|:--:|
|10.0.0.1|root|password|

### APP
- PassWall
- ~luci-app-udpxy~
- ~igmpproxy~


### NGINX错误修复方式

需要删除原`/etc/nginx/uci.conf`,然后新建一个uci.conf内容见下

```
worker_processes auto;
user root;
events {}
http {
        access_log off;
        log_format openwrt
                '$request_method $scheme://$host$request_uri => $status'
                ' (${body_bytes_sent}B in ${request_time}s) <- $http_referer';
        include mime.types;
        default_type application/octet-stream;
        sendfile on;
        client_max_body_size 128M;
        large_client_header_buffers 2 1k;
        gzip on;
        gzip_vary on;
        gzip_proxied any;
        root /www;
        server {
                listen 80;
                listen [::]:80;
                server_name _lan;
                include restrict_locally;
                include conf.d/*.locations;
                access_log off; # logd openwrt;
        }
        include conf.d/*.conf;
}
```
## License

[MIT](https://github.com/P3TERX/Actions-OpenWrt/blob/main/LICENSE) © P3TERX
