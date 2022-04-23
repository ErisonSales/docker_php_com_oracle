# Ambiente docker PHP configurado para conexões com Oracle Database, SQL Server e Ldap

## Contem neste ambiente
- git : latest
- composer : latest
- npm : latest
- php 7.4-fpm

## Extensções habilitadas:
> ldap, mysql, pdo_mysql, sqlsrv, pdo_sqlsrv, oci8, pdo_oci, intl, sodium

## Variáveis e configurações do ambiente
> No arquivo .env você podera configurar as variáveis de ambiente como nome, senhas, e portas para o banco mysql e servico web:

- PROJECT_NAME=nome_projeto
- DB_PORT=3310 (mude a porta conforme necessidade)
- DB_USERNAME=nome_usuario
- DB_DATABASE=nome_banco
- DB_PASSWORD=sua_senha
- APP_DIRECTORY=sua_aplicação
- APP_PORT=8000 (mude a porta conforme necessidade)


* Após montado o ambiente, será criado um diretório na raiz do projeto em: ./app/sua_aplicação
* Será necessário conceder permissão neste diretório para utiliza-lo em seu editor de codigo pi IDE.


**Linux e MacOS**
````
sudo chown ${USER}:${USER} app -R
````

**Windows**
````
Se utiliza o WSL configure pelo linux instalado, caso contrário altere as permisções em propriedades da pasta.
````


## Iniciando o ambiente:
**Iniciando o ambiente**
```
docker-compose -f "docker-compose.yml" up -d --build
```

**Parando o ambiente**
````
docker-compose -f "docker-compose.yml" down
````

**Acessando terminal do ambiente**
````
docker exec -it nome_projeto-app bash
````


## Notas para atualização
* oci_extension : Documentação de instalação ou atualização disponível em [https://pecl.php.net/package/oci8](https://pecl.php.net/package/oci8)

* sqlsrv_extension : Documentação de instalação ou atualização disponível em: [https://docs.microsoft.com](https://docs.microsoft.com/en-us/sql/connect/php/download-drivers-php-sql-server?view=sql-server-ver15#previous-releases)