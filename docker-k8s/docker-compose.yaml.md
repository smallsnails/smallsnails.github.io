```yaml
version: "3"
services:
  nodejs:
    image: node:18.20.3
    container_name: nodejs
    #environment:
    #  - NODE_OPTIONS=--max_old_space_size=8192
    ports:
      - 8080:8080
    volumes:
      - ./html:/usr/share/nginx/html
    #working_dir: /usr/share/nginx/html/xxx
    #command:
    #  - npm run dev
    tty: true
    privileged: true
  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - 8000:8000
      - 8001:8001
      - 8002:8002
      - 8003:8003
      - 8004:8004
      - 8005:8005
    volumes:
      - ./nginx-vhost:/etc/nginx/conf.d
      - ./html:/usr/share/nginx/html
  redis:
    image: redis:latest
    container_name: redis
    ports:
      - 6379:6379
    volumes:
      - ./redis/redis.conf:/etc/redis/redis.conf
      - ./redis/data:/data
  mysql:
    image: mysql
    container_name: mysql8
    environment:
      MYSQL_ROOT_PASSWORD: "root"
    ports:
      - 3306:3306
    volumes:
      #- ./mysql/mysql.cnf:/etc/mysql/conf.d/mysql.cnf
      - ./mysql/data:/var/lib/mysql
  php:
    image: php:7.4.33-fpm
    container_name: php74
    privileged: true
    ports:
      - 9000:9000
    volumes:
      #- ./php/php.ini:/usr/local/etc/php/php.ini
      - ./html:/usr/share/nginx/html
    depends_on:
      - nginx
      - mysql
    links:
      - nginx
      - mysql
  php71:
    #image: php:7.1.33-fpm
    image: php:7.1.20-fpm
    container_name: php71
    privileged: true
    ports:
      - 9100:9000
    volumes:
      #- ./php/php.ini:/usr/local/etc/php/php.ini
      - ./html:/usr/share/nginx/html
    depends_on:
      - nginx
      - mysql
    links:
      - nginx
      - mysql
```

> 自动分配网络
```yaml
version: "3"
services:
  mysql:
    image: mysql:latest
    container_name: mysql9
    environment:
      MYSQL_ROOT_PASSWORD: "root"
    ports:
      - 3306:3306
    volumes:
      #- ./mysql.cnf:/etc/mysql/conf.d/mysql.cnf
      - ./data:/var/lib/mysql
```


> 指定网络
```yaml
version: "3"
services:
  mysql:
    image: mysql:latest
    container_name: mysql9
    environment:
      MYSQL_ROOT_PASSWORD: "root"
    ports:
      - 3306:3306
    volumes:
      #- ./mysql.cnf:/etc/mysql/conf.d/mysql.cnf
      - ./data:/var/lib/mysql
    networks:
      mysql9:
        ipv4_address: 172.20.0.2
networks:
  mysql9:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/16
```


> 引用已有的网络
```yaml
version: "3"
services:
  mysql:
    image: mysql:latest
    container_name: mysql9
    environment:
      MYSQL_ROOT_PASSWORD: "root"
    ports:
      - 3306:3306
    volumes:
      #- ./mysql.cnf:/etc/mysql/conf.d/mysql.cnf
      - ./data:/var/lib/mysql
    networks:
      - hyperf_default
networks:
  hyperf_default:
    external: true
```

