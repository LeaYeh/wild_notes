# Nginx

## What is Nginx

* Nginx is a web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache.
* Nginx is known for its high performance, stability, rich feature set, simple configuration, and low resource consumption.

## Others Solutions for Web Servers

* Apache
* Lighttpd
* Caddy
* OpenLiteSpeed
* Hiawatha
* Cherokee
* LiteSpeed Web Server
* Tengine

## Why Nginx (Comparison with Others)

1. Nginx vs. Apache
    - Performance: Nginx is known for its high performance and low resource consumption, particularly under high loads. It uses an event-driven architecture, which makes it more scalable and capable of handling many connections simultaneously with low memory usage.
    - Configuration: Nginx configuration files are typically simpler and more concise compared to Apache’s. This makes it easier to manage for large-scale deployments.
    - Static Content: Nginx excels at serving static content quickly due to its efficient design, often outperforming Apache in this area.
    - Reverse Proxy: Nginx is widely used as a reverse proxy server, load balancer, and HTTP cache, thanks to its robust and easy-to-configure proxy capabilities.
2. Nginx vs. Lighttpd
    - Performance and Scalability: While Lighttpd is also designed for high performance, Nginx generally outperforms it in scalability and stability under heavy load.
    - Community and Support: Nginx has a larger community and more comprehensive documentation, which can be a critical factor for troubleshooting and learning.

## How to install Nginx

### Ubuntu

```bash
sudo apt update
sudo apt install nginx
```

## How to configure Nginx

### Configuration File

* The main configuration file for Nginx is located at `/etc/nginx/nginx.conf`.
* The configuration file is divided into multiple sections, each of which can contain directives that define how Nginx should behave.
* The main sections in the configuration file are:
    - `events`: This section defines how Nginx should handle connections and events.
    - `http`: This section defines the HTTP server configuration.
    - `server`: This section defines the configuration for a specific server block.
    - `location`: This section defines the configuration for a specific location block.


### Server Blocks

* Nginx uses server blocks to determine how to process requests for a specific domain or IP address.

```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    root /var/www/html;
    index index.html;
}
```

* In this example, the server block listens on port 80 for requests to `example.com` and `www.example.com`. It serves files from the `/var/www/html` directory and uses `index.html` as the default index file.


# Security Protocol - TLS/SSL

## What is TLS/SSL

* TLS (Transport Layer Security) and its predecessor SSL (Secure Sockets Layer) are cryptographic protocols that provide secure communication over a computer network.
* TLS and SSL encrypt the data transmitted between two systems, ensuring that it cannot be intercepted or tampered with by attackers.
* TLS and SSL are commonly used to secure web traffic, email communication, and other network services.

## Why Web Servers Need TLS/SSL

* Protecting Sensitive Data: TLS/SSL encrypts data transmitted between a web server and a client, preventing eavesdropping and data theft.
* Authentication: TLS/SSL allows web servers to prove their identity to clients, ensuring that users are connecting to the correct server and not an imposter.
* Trust: TLS/SSL certificates are issued by trusted Certificate Authorities (CAs), providing assurance to users that they are connecting to a legitimate website.

## How to Enable TLS/SSL in Nginx

### What is a SSL Certificate

* An SSL certificate is a digital certificate that authenticates the identity of a website and encrypts data transmitted between the website and the user's browser.

### Step 1: Obtain an SSL Certificate

* You can obtain an SSL certificate from a trusted Certificate Authority (CA) or generate a self-signed certificate for testing purposes.

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
```

* `openssl`: It is used to generate the SSL and TLS certificates.
* `-x509`: This option outputs a self-signed certificate instead of a certificate signing request (CSR).
* `-nodes`: This option specifies that the private key should not be encrypted with a password.
* `-days 365`: This option sets the validity period of the certificate to 365 days.
* `-newkey rsa:2048`: This option specifies that a new RSA key pair should be generated with a key length of 2048 bits.
* The private key is stored in `/etc/ssl/private/nginx-selfsigned.key` and the certificate is stored in `/etc/ssl/certs/nginx-selfsigned.crt`.

### Step 2: Configure Nginx to Use SSL

* Update the Nginx configuration file to use the SSL certificate and key.

```nginx
server {
    listen 443 ssl;
    server_name example.com www.example.com;
    root /var/www/html;
    index index.html;

    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384';
    ssl_prefer_server_ciphers off;
}
```

* `server { ... }`: This block defines the configuration for a specific server.
* `listen 443 ssl`: This directive tells Nginx to listen on port **443** (**the default port for HTTPS**) and use SSL.
* `server_name example.com www.example.com`: This directive specifies the domain names that this server block should respond to.
* `root /var/www/html`: This directive sets the root directory for serving files.
* `index index.html`: This directive specifies the default index file.
* `ssl_certificate`: This directive specifies the path to the SSL certificate file.
* `ssl_certificate_key`: This directive specifies the path to the SSL private key file.
* `ssl_protocols`: This directive specifies the TLS protocols that should be used.
* `ssl_ciphers`: This directive specifies the ciphers that should be used for encryption.
    * The ciphers listed here are considered secure and provide strong encryption.
    * `ssl_prefer_server_ciphers off`: This directive disables the server's preference for ciphers, allowing the client to choose the cipher.

### Step 3: Restart Nginx

```bash
sudo systemctl restart nginx
```

## How to Redirect HTTP to HTTPS

* You can configure Nginx to redirect HTTP traffic to HTTPS to ensure that all communication is encrypted.

```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    return 301 https://$server_name$request_uri;
}
```

* In this example, the server block listens on port 80 and redirects all traffic to the HTTPS version of the site.
* The `return 301` directive issues a **permanent redirect** (HTTP status code 301) to the HTTPS version of the site.

## How to combine WordPress with Nginx

* WordPress is a popular content management system (CMS) that can be used to create websites and blogs.
* You can combine WordPress with Nginx to create a high-performance and secure web server environment.

### Step 1: Install WordPress

* Download and install WordPress on your server.

```bash
cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzvf latest.tar.gz
sudo chown -R www-data:www-data /var/www/html/wordpress
```

### Step 2: Configure Nginx for WordPress

* Update the Nginx configuration file to serve WordPress.

```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    root /var/www/html/wordpress;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
    }
}
```

* In this example, the server block listens on port 80 and serves WordPress from the `/var/www/html/wordpress` directory.
* The `location /` block handles requests for WordPress pages and passes them to the PHP FastCGI process.
    * What is FastCGI: FastCGI is a protocol for interfacing web servers with external applications. It is a variation on the earlier Common Gateway Interface (CGI).
* The `location ~ \.php$` block processes PHP files using the PHP FastCGI process.
* The `include snippets/fastcgi-php.conf;` directive includes the configuration for handling PHP files.

