# rebrain_prometheus
sudo apt-get install git curl &&  curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh ./get-docker.sh --dry-run && sudo sh get-docker.sh

Или так


apt update && apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - &&  add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" && apt update && apt-cache policy docker-ce && apt-get install docker-ce -y && systemctl status docker && apt-get update && apt-get install docker-compose-plugin -y



Композ


sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose && docker-compose --version



sudo mkdir db_data && sudo mkdir wordpress_data











nano docker-compose.yml

version: "3.8"

services:
  cadvisor:
    image: google/cadvisor:latest
    container_name: cadvisor
    ports:
      - "8080:8080"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:rw
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro

Не работает!
-------
  mysqld-exporter:
    image: quay.io/prometheus/mysqld-exporter
    container_name: mysqld-exporter
    volumes fvdfvrwtrvrte
    ports:
     - "9104:9104"
    command:
     - "--mysqld.username=wordpress:YOUR_DB_PASSWORD"
     - "--mysqld.address=localhost:3306"
------------




  node_exporter:
    image: prom/node-exporter:latest
    container_name: node_exporter
    ports:
      - "9100:9100"

  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    ports:
     - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: YOUR_ROOT_DB_PASSWORD
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: YOUR_DB_PASSWORD

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    volumes:
      - wordpress_data:/var/www/html
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: YOUR_DB_PASSWORD
      WORDPRESS_DB_NAME: wordpress

volumes:
  db_data: {}
  wordpress_data: {}








второй хост


docker run -d --name=grafana -p 3000:3000 grafana/grafana



nano prometheus.yml

scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['167.71.8.139:9100']

  - job_name: 'cadvisor'
    static_configs:
      - targets: ['167.71.8.139:8080']

  - job_name: 'mysqld_exporter'
    static_configs:
      - targets: ['167.71.8.139:9104']









nano docker-compose.yml

version: "3.8"
services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    restart: always



dashboards

893
1860
14727
![image](https://github.com/mltop-23/rebrain_prometheus/assets/87617943/b290709a-915a-4e3b-9034-9307a092e7e2)
