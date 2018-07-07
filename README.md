# WooCommerce Docker Local Environment With SSL

Creates a local development environment powered by Docker and the [Woodby Docker4WordPress](https://wodby.com/stacks/wordpress/docs/local/) project. Includes a wealth of useful tools, including Mailhog for capturing emails, PhpMyAdmin for managing MariaDB database, Portainer for managing Docker, Redis for caching, and Xdebug for stack tracing. Powered by NginX, PHP7.2, Redis caching and SSL to best reflect live environments.

## Prerequisites

Install and launch Docker Community Edition onto your workstation, if not already available.

[Download Docker CE from this link](https://store.docker.com/search?type=edition&offering=community)

## Setting It Up

* Download this project to a folder on your workstation.
* Add the URLs to your `/etc/hosts` file:
```
sudo nano /etc/hosts
127.0.0.1 localhost mailhog phpmyadmin portainer
```
* Make an SSL certificate using one of these methods, store `localhost.key` and `localhost.crt` into the folder `certs/` within your project
* Option 1 - Steps to make a self-signed certificate:
```
openssl genrsa -des3 -passout pass:x -out localhost.pass.key 2048;
openssl rsa -passin pass:x -in localhost.pass.key -out localhost.key;
rm localhost.pass.key;
openssl req -new -key localhost.key -out localhost.csr;
openssl x509 -req -sha256 -days 365 -in localhost.csr -signkey localhost.key -out localhost.crt;
```
* Option 2 - Steps to obtain an SSL certificate from LetsEncrypt.org:
```
sudo add-apt-repository ppa:certbot/certbot;
sudo apt update;
sudo apt install certbot;
sudo certbot certonly --manual -d *.mydomain.com --agree-tos --no-bootstrap --manual-public-ip-logging-ok --preferred-challenges dns-01 --server https://acme-v02.api.letsencrypt.org/directory;
```
* If you're using a self-signed certificate, add it into your OS's Keychain App and set it to Always Trust to prevent browsers throwing security warnings.
* Run `docker-compose up -d` to have docker download the images and bring your system online.
* Point your browser to `https://localhost/` and set-up WordPress and WooCommerce to your liking.

### Further Notes

Access to tools:
* `http://mailhog/`
* `http://phpmyadmin/`
* `http://portainer/`

Docker4WordPress commands (execute from inside project folder):
* `docker-compose stop` to stop the services
* `docker-compose logs` to view logs
* `docker-compose exec php sh` to shell into the container

You can set-up multiple instances by creating multiple project folders containing the `docker-compose.yml` file and bringing up a `traefik.yml` file in the parent folder. I recommend taking the simpler approach whenever possible. Note that folder names matter as they are used for the container naming. [Instructions can be found here](https://wodby.com/stacks/wordpress/docs/local/multiple-projects/).

If you want Redis object cache to run, install a caching plugin that uses Redis and add this to your `wp-config.php` file: `define( 'WP_REDIS_HOST', 'redis' );`

## Author

* **Coded Commerce, LLC** - *Initial work* - [Coded-Commerce-LLC](https://github.com/Coded-Commerce-LLC)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
