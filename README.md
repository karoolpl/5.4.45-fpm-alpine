A **PHP 5.4.45 alpine** image refer to official **PHP 5.5.34 alpine** Dockerfile.

- [Project on Docker Hub](https://hub.docker.com/r/yeszao/php)
- [Project on GitHub](https://github.com/yeszao/5.4.45-fpm-alpine)
- [How to use PHP image](https://hub.docker.com/_/php)

Installation package verified with [php-5.4.45.tar.gz sha256](https://www.php.net/releases/#5.4.45) and [PHP 5.4 GPG](https://www.php.net/gpg-keys.php#gpg-5.4).

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

# Patches

To build PHP 5.4.45 on alpine 3.8, we must do the following 2 steps.
1. Replace **config.guess** and **config.sug** file to the source code root, which make PHP 5.4 to identify Alpine 3.8. Otherwise we may get this error:
    ```bash
    checking for egrep... /bin/grep -E
    checking for a sed that does not truncate output... /bin/sed
    checking build system type... Invalid configuration `x86_64-linux-musl': system `musl' not recognized
    configure: error: /bin/sh ./config.sub x86_64-linux-musl failed
    ```
2. Patch the [SSLv3](https://cvsweb.openbsd.org/ports/lang/php/5.4/patches/patch-ext_openssl_xp_ssl_c) and [EGD](https://cvsweb.openbsd.org/ports/lang/php/5.4/patches/patch-ext_openssl_openssl_c) patches (Both from [https://www.libressl.org/patches.html](https://www.libressl.org/patches.html)) to avoid SSLv3 error:
    ```bash
    ext/openssl/openssl.o: In function `zif_openssl_encrypt':
    openssl.c:(.text+0x7a2f): warning: EVP_EncryptFinal is often misused, please use EVP_EncryptFinal_ex and EVP_CIPHER_CTX_cleanup
    ext/openssl/openssl.o: In function `zif_openssl_decrypt':
    openssl.c:(.text+0x7d7b): warning: EVP_DecryptFinal is often misused, please use EVP_DecryptFinal_ex and EVP_CIPHER_CTX_cleanup
    ext/openssl/openssl.o: In function `php_openssl_load_rand_file':
    openssl.c:(.text+0x215b): undefined reference to `RAND_egd'
    ext/openssl/xp_ssl.o: In function `php_openssl_sockop_set_option':
    xp_ssl.c:(.text+0x11a7): undefined reference to `SSLv3_server_method'
    xp_ssl.c:(.text+0x11fd): undefined reference to `SSLv3_client_method'
    collect2: error: ld returned 1 exit status
    make: *** [Makefile:247: sapi/cli/php] Error 1
    make: *** Waiting for unfinished jobs....
    ext/openssl/openssl.o: In function `zif_openssl_encrypt':
    openssl.c:(.text+0x7a2f): warning: EVP_EncryptFinal is often misused, please use EVP_EncryptFinal_ex and EVP_CIPHER_CTX_cleanup
    ext/openssl/openssl.o: In function `zif_openssl_decrypt':
    openssl.c:(.text+0x7d7b): warning: EVP_DecryptFinal is often misused, please use EVP_DecryptFinal_ex and EVP_CIPHER_CTX_cleanup
    ext/openssl/openssl.o: In function `php_openssl_load_rand_file':
    openssl.c:(.text+0x215b): undefined reference to `RAND_egd'
    ext/openssl/xp_ssl.o: In function `php_openssl_sockop_set_option':
    xp_ssl.c:(.text+0x11a7): undefined reference to `SSLv3_server_method'
    xp_ssl.c:(.text+0x11fd): undefined reference to `SSLv3_client_method'
    collect2: error: ld returned 1 exit status
    make: *** [Makefile:260: sapi/fpm/php-fpm] Error 1
    ```


# Reference
- [Dockerfile of PHP 5.6.40 alpine3.8](https://github.com/docker-library/php/blob/ecd7dcc69c76f0e582c2274ecb90dcb7264e245c/5.6/alpine3.8/fpm/Dockerfile)
- [Dockerfile of PHP 5.4.45 jessie](https://github.com/docker-library/php/blob/995bf17c9ded3d622f6b9efb902756562538ab13/5.4/fpm/Dockerfile)
