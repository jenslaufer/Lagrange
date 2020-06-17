# How to setup a free LetsEncrypt SSL certificate with the ZeroSSL Docker Image


- Docker image https://hub.docker.com/r/zerossl/client/

```shell
docker pull zerossl/client

```

alias le.pl='docker run -it -v /home/jens/keys_and_certs:/data -v /home/jens/public_html/.well-known/acme-challenge:/webroot -u $(id -u) --rm zerossl/client'