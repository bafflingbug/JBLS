### LicenseServer
[lanyu大佬的搭建教程博客](http://blog.lanyus.com/archives/174.html)
#### 使用帮助
1. 复制JB文件夹到/data目录下
2. 打开`/etc/rc.local` (添加开机自启动)

        vi /etc/rc.local

3. 添加

        cd /home/JB
        nohup ./IntelliJIDEALicenseServer_linux_amd64 -prolongationPeriod 999999999999 > log.out 2>&1 &

4. 配置Nginx反向代理

        server {
            listen       80;
            server_name  example.com;
            
            location / {
                proxy_redirect off;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://127.0.0.1:1027;
                proxy_redirect http://127.0.0.1:1027 http://example.com;
            }
            location /static/  { 
                root  /home/JB/;
                expires      7d; 
            } 
        }

5. 重启Nginx

        service nginx restart
