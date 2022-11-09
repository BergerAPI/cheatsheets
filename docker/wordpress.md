Wordpress is a free and open-source content-management-system (CMS). Many websites on the internet run it, due to its ease of use. This example will show how to create and deploy a Compose-File to run Wordpress.

## Creating the Compose-File
The compose file will deploy MariaDB and Wordpress.

```yaml
services:
  db:
    image: mariadb:10.6.4-focal
    command: '--default-authentication-plugin=mysql_native_password'
    volumes:
      - ./data/mysql:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
    expose:
      - 3306
  wordpress:
    image: wordpress:latest
    ports:
      - 80:80
    restart: always
    volumes:
	  - ./data/wordpress:/var/www/html
    environment:
      - WORDPRESS_DB_HOST=db:3306
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=
      - WORDPRESS_DB_NAME=wordpress
```

Make sure to change the password, even though the database won't be exposed.