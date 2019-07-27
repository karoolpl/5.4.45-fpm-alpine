A **PHP 5.4.45 alpine** image refer to official **PHP 5.5.34 alpine** Dockerfile.

- [Project on Docker Hub](https://hub.docker.com/r/yeszao/php)
- [Project on GitHub](https://github.com/yeszao/5.4.45-fpm-alpine)
- [How to use PHP image](https://hub.docker.com/_/php)

# How to use this image
Create a Dockerfile in your PHP project
```dockerfile
FROM yeszao/php:5.4.45-fpm-alpine
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp
CMD [ "php", "./your-script.php" ]
```

Then, run the commands to build and run the Docker image:
```bash
$ docker build -t my-php-app .
$ docker run -it --rm --name my-running-app my-php-app
```

Run a single PHP script
For many simple, single file projects, you may find it inconvenient to write a complete Dockerfile. In such cases, you can run a PHP script by using the PHP Docker image directly:
```bash
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp yeszao/php:5.4.45-fpm-alpine php your-script.php
```

# Reference
- [Dockerfile of PHP 5.5.34 alpine](https://github.com/docker-library/php/blob/995bf17c9ded3d622f6b9efb902756562538ab13/5.4/fpm/Dockerfile)
- [Dockerfile of PHP 5.6.20 alpine](https://github.com/docker-library/php/blob/4677ca134fe48d20c820a19becb99198824d78e3/5.6/fpm/Dockerfile)
- [Dockerfile of PHP 5.4.45 jessie](https://github.com/docker-library/php/blob/995bf17c9ded3d622f6b9efb902756562538ab13/5.4/fpm/Dockerfile)
