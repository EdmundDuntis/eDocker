```bash
docker pull ubuntu:18.04
docker run -it --rm \
    ubuntu:18.04 \
    bash

docker image ls
docker system df

docker image ls -a
docker image ls ubuntu
docker image ls ubuntu:18.04
docker image ls --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"

```





```bash
docker run --name webserver -d -p 80:80 nginx

docker exec -it webserver bash
  echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
  exit
docker diff webserver

docker commit \
    --author "Roger <edmund_duntis@163.com>" \
    --message "修改默认网页" \
    webserver \
    nginx:v2
    
docker run --name web2 -d -p 81:80 nginx:v2

docker run --name web3 -d -p 82:80 nginx:v3

```

