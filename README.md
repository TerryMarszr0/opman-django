# OpMan2.0

## 运行环境介绍 ##

系统：Mac

软件：Python3.6,Django1.11.3,MySQL5.7

## 全新UI ##

![](https://github.com/hgz6536/hgz6536.github.io/blob/master/images/OpMan2.0.png)

## 部署方法 ##

- 安装MySQL5.7,并设置my.cnf

`character-set-server = utf8`

`character-set-client-handshake = FALSE`

`sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER`

- 克隆代码

`cd /data/webroot`

`git clone https://github.com/hgz6536/opman-django.git`

`cd opman-django && pip3 install -r requirements.txt`

- 初始化项目

`python manage.py migrate`

`python manage.py createsuperuser`

`/usr/bin/python manage.py celery worker --loglevel=info -E -c 2 &`

- 安装uwsgi

`pip3 install uwsgi`

- nginx配置文件

upstream opman {

        server 127.0.0.1:8000;
}

server {

        listen 80;
        server_name opman.niubilety.com
        charset utf-8;

        gzip on;
        gzip_min_length 1000;
        gzip_buffers 4 16k;
        gzip_http_version 1.1;
        gzip_comp_level 3;
        gzip_vary on;

        client_max_body_size 8M;

        #access_log /data/logs/nginx/opman_access.log;
        #error_log /data/logs/nginx/opman_error.log;

        location /medis {
                alias /data/webroot/opman-django/media;
        }

        location /static {
                alias /data/webroot/opman-django/static;
        }

        location / {
                uwsgi_pass opman;
                include /etc/nginx/uwsgi_params;
        }

}

- 启动uwsgi

`uwsgi --ini /data/webroot/opman-django/uwsgi.ini`

- 平滑重启uwsgi

`uwsgi --reload /tmp/opman.pid`

- 启动nginx

`/etc/init.d/nginx start`

## 本项目交流群 ##
`580838402`

如果困惑直击QQ
<a target="_blank" href="http://wpa.qq.com/msgrd?v=3&uin=847644968&site=qq&menu=yes">
     <img border="0" src="http://wpa.qq.com/pa?p=2:847644968:52" alt="点击这里给我发消息" title="点击这里给我发消息"/>
</a>
