# docker-keycloak

##Instalação docker ubuntu

```
 curl -fsSL https://get.docker.com -o get-docker.sh
 sudo sh get-docker.sh   
```

##Instalar docker compose
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

## configuração do nginx

```
location /some/path/ {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_pass http://localhost:8000;
}
````
