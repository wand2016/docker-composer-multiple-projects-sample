# 複数案件をまたいで開発しているとき、ホスト側port番号を考えるのに疲れたあなたへのサンプル #

/etc/hosts

```
...
127.0.0.1 anken-1
127.0.0.2 anken-2
...
```

up and run

```
$ cd ./anken-1
$ docker-compose up -d

$ cd ../anken-2
$ docker-compose up -d

docker ps
```

```
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                 NAMES
36d5b663edf6        mysql:5.7           "docker-entrypoint.s…"   3 seconds ago       Up 2 seconds        127.0.0.2:3306->3306/tcp, 33060/tcp   anken-2_db_1
c347fe183ad6        httpd:2.4           "httpd-foreground"       3 seconds ago       Up 2 seconds        127.0.0.2:80->80/tcp                  anken-2_web_1
8ad0a5ae018d        mysql:5.7           "docker-entrypoint.s…"   3 seconds ago       Up 2 seconds        33060/tcp, 127.0.0.2:3307->3306/tcp   anken-2_secure-db_1
6955a89c43d9        httpd:2.4           "httpd-foreground"       29 seconds ago      Up 28 seconds       127.0.0.1:80->80/tcp                  anken-1_web_1
f13161e0dfef        mysql:5.7           "docker-entrypoint.s…"   29 seconds ago      Up 28 seconds       33060/tcp, 127.0.0.1:3307->3306/tcp   anken-1_secure-db_1
c1905356e589        mysql:5.7           "docker-entrypoint.s…"   29 seconds ago      Up 28 seconds       127.0.0.1:3306->3306/tcp, 33060/tcp   anken-1_db_1
```

IP addresses style

```
$ curl 127.0.0.1:80
<html>案件1</html>

$ curl 127.0.0.2:80
<html>案件2</html>

curl 127.0.0.2:3306
Warning: Binary output can mess up your terminal. Use "--output -" to tell 
Warning: curl to output it to your terminal anyway, or consider "--output 
Warning: <FILE>" to save to a file.
```

hostname style

```
$ curl anken-1:80
<html>案件1</html>

$ curl anken-2:80
<html>案件2</html>
```

:tada:
