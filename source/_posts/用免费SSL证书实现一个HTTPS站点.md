---
title: 用免费SSL证书实现一个HTTPS站点
date: 2018-11-22 16:29:35
categories: Linux
tags: https
---

环境阿里云centOS服务器

#### 1.安装python2-certbot-nginx

        yum install python2-certbot-nginx
   
安装python2-certbot-nginx，会报错没有找到安装包可以先执行 epel-release

        yum install epel-release

        yum install python2-certbot-nginx


#### 2.certbot工具配置 

        certbot --nginx --nginx-server-root=/etc/nginx/nginx.conf -d 域名 (申请证书的域名)

可能会出现问题：

        ImportError: No module named 'requests.packages.urllib3'


解决方法： 
        pip install --upgrade --force-reinstall 'requests==2.6.0' urllib3

然后在进行certbot配置会出现：

        1: No redirect - Make no further changes to the webserver configuration.
        2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
        new sites, or if you're confident your site works on HTTPS. You can undo this
        change by editing your web server's configuration.
        - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
        Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 1

选择1不重定向
选择2则所有该域名下站点都会重定向到https

在不确定你的服务端所有的接口都支持https的时候，选择1

成功后发现想要配置文件中多了：

        listen 443 ssl; # managed by Certbot
        ssl_certificate /etc/letsencrypt/live/backstage.qingyun100.cn/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/backstage.qingyun100.cn/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

首先要确保443端口的开放，再去https访问域名，会看到安全访问标志

        firewall-cmd --zone=public --add-port=443/tcp --permanent
        firewall-cmd --reload

如果是阿里云的服务器要在安全组里面设置开放的端口。

#### 3.自动更新证书    

由于这个证书的时效只有90天，我们需要设置自动更新的功能，帮我们自动更新证书的时效

先在命令行模拟证书更新：

        sudo certbot renew --dry-run

模拟更新成功的效果如下：

        - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
        ** DRY RUN: simulating 'certbot renew' close to cert expiry
        **          (The test certificates below have not been saved.)

        Congratulations, all renewals succeeded. The following certs have been renewed:
        /etc/letsencrypt/live/backstage.qingyun100.cn/fullchain.pem (success)
        ** DRY RUN: simulating 'certbot renew' close to cert expiry
        **          (The test certificates above have not been saved.)
        - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

既然模拟成功，我们就使用crontab -e的命令来启用自动任务，命令行：

        sudo crontab -e

添加配置：

        30 2 * * 1 /usr/bin/certbot renew >> /var/log/le-renew.log

上面的执行时间为：每周一半夜2点30分执行renew任务。

你可以在命令行执行

        /usr/bin/certbot renew >> /var/log/le-renew.log
        
看看是否执行正常，如果一切OK，那么我们的配置到此结束！

现在你就可以使用免费的ssl进行安全访问啦