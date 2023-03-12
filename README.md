# Docker Compose WordPress
This is a repository for building WordPress that supports SSL/TLS with Docker Compose.
Used in combination with [sanjeetsagar/docker-nginx-proxy-with-auto-ssl](https://github.com/sanjeetsagar/docker-nginx-proxy-with-auto-ssl).

## Required operating environment
- Docker `>= 20.10`
- Docker Compose `>= 2.7`

Reference: [Compose file - Docker documentation](https://matsuand.github.io/docs.docker.jp.onthefly/compose/compose-file/)

## How to use

### Initial setting
Write your WordPress and reverse proxy settings in the `.env` file.

- `DB_PASSWORD`: Database password
- `PRIMARY_HOST`: primary hostname
   Create a container or network with this name.
- `HOST`: List of host names that can connect to WordPress
   You can specify multiple options by separating them with commas.
- `EMAIL`: Email address to be contacted when SSL/TLS certificates are about to expire

#### Example `.env` file
```
DB_PASSWORD=password
PRIMARY_HOST=www.example.com
HOST=www.example.com,example.com
EMAIL=mail@example.com
```

### Start up
Start [sanjeetsagar/docker-nginx-proxy-with-auto-ssl](https://github.com/sanjeetsagar/docker-nginx-proxy-with-auto-ssl) first.

Start it with the following command.
The URL that you connect to when performing the initial settings is automatically recognized as the default URL for WordPress, so **be sure to connect with the main host name after startup** and perform the initial settings for WordPress.

``` bash
docker-compose up -d
```

## Frequently Asked Questions (FAQ)
### Q. I want to automatically redirect HTTP to HTTPS.
nginx-proxy will do it for you, so you don't need to change the conventional `.htaccess`.

### Q. I want to automatically redirect from without www to with www. Or vice versa.
WordPress seems to automatically redirect to one URL.
However, the URL that will be redirected to in this case will be the **URL that was connected during the initial setup**, so be sure to connect with the main URL during the initial setup.

For example, when you can connect to WordPress from `www.example.com` (with www) and `example.com` (without www), if you want to unify the URL to `www.example.com`, use `www. If you want to make initial settings with example.com` and unify the URL to `example.com`, please do the initial settings with `example.com`.

## Reference
### Docker Hub
- [wordpress](https://hub.docker.com/_/wordpress)
- [mariadb](https://hub.docker.com/_/mariadb)