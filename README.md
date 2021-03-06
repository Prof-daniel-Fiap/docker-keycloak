# docker-keycloak

## Instalação docker ubuntu

```
 curl -fsSL https://get.docker.com -o get-docker.sh
 sudo sh get-docker.sh   
```
reinicia terminal
```
sudo usermod -a -G docker ubuntu
```

## Instalação docker compose
```
sudo apt-get install docker-compose
```

Projeto para subir um identity manager com docker compose


```version: '2'
services:
  keycloak:
    image: jboss/keycloak:10.0.0
    command:
      [
        '-b',
        '0.0.0.0',
        '-Dkeycloak.migration.action=import',
        '-Dkeycloak.migration.provider=dir',
        '-Dkeycloak.migration.dir=/opt/jboss/keycloak/realm-config',
        '-Dkeycloak.migration.strategy=OVERWRITE_EXISTING',
        '-Djboss.socket.binding.port-offset=1000',
        '-Dkeycloak.profile.feature.upload_scripts=enabled',
      ]
    volumes:
      - ./realm-config:/opt/jboss/keycloak/realm-config
    environment:
      - KEYCLOAK_USER=admin
      - KEYCLOAK_PASSWORD=Fiap2015
      - DB_VENDOR=h2
      - PROXY_ADDRESS_FORWARDING=true
    ports:
      - 9080:9080
      - 9443:9443
      - 10990:10990
 ```
 
 
## Subir container keycloak (docker compose)

```
docker-compose up -d
docker-compose stop
docker ps
```

## configurar dominio gratis no noip apontando ip publico ec2

Cadastre-se em:
https://www.noip.com/login?ref_url=console


## Configuração do nginx

Instalar o nginx no ubuntu:
```
sudo apt-get install nginx
```


Obter IP do docker:

```
sudo apt install net-tools
ifconfig
```

 Mudar a configuração do nginx /etc/nginx/sites-enabled/default


```
server_name XXXXXXXXX; # obter do NO-IP




    location /auth {
      proxy_pass http://localhost:9080;


        proxy_set_header X-Forwarded-For $proxy_protocol_addr; # To forward the original client's IP address
        proxy_set_header X-Forwarded-Proto $scheme; # to forward the  original protocol (HTTP or HTTPS)
        proxy_set_header Host $host; # to forward the original host requested by the client

    }


````

Instalar certbot:

https://certbot.eff.org/lets-encrypt/ubuntuxenial-nginx.html

```
sudo snap install core; 
sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
Criar certificado no nginx:
```
sudo certbot --nginx
```
