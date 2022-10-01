> ## DevOps Tools & Cloud Computing - João Carlos Menk

## Arquitetura 2º Checkpoint: Projeto DimDim
<img align="center" alt="BeLive-HTML" src="https://i.imgur.com/5sSmpRf.png">

### Obs: Esse projeto foi baseado no código e matéria de Digital Business Enablement 

# ACR e ACI
### Get DimDim

[GET] - http://epictaskcheckpointacr.eastus.azurecontainer.io:8080/api/task/




### Post DimDim

[POST] - http://epictaskcheckpointacr.eastus.azurecontainer.io:8080/api/task/

```diff
- (* required)
```
- [x] Username
- [x] Password

```
{
	"username" : "joao@fiap.com.br",
	"password" : "123"
}
```

```
{
      "title": "Modelar o BD",
      "description": "Modelar as tabelas do banco",
      "score": 100
}
```

### Del DimDim

[DEL] - http://epictaskcheckpointacr.eastus.azurecontainer.io:8080/api/task/idBD



### Put DimDim

[PUT] - http://epictaskcheckpointacr.eastus.azurecontainer.io:8080/api/task/idBD
```
{
      "title": "BeLive CP2",
      "description": "Banco de Dados Azure",
      "score": 100
}
```
## Passo a Passo dos comandos para criação dos recursos Azure

```
==> Banco de Dados <==

az group create --name rg-checkpoint --location eastus

```

```
==> Criar  o servidor PaaS Azure SQL <==
az sql server create -l eastus -g rg-checkpoint -n sqlserver-rm88233 -u admsql -p devops@fiap21 --enable-public-network true
```
```
==> Criar o banco de dados pythondb <==
az sql db create -g rg-checkpoint -s sqlserver-rm88233 -n pythondb --service-objective Free --backup-storage-redundancy Local --zone-redundant false
```

```
==> Criar uma regra para deixar todos os IPs acessarem o Banco <==
az sql server firewall-rule create -g rg-checkpoint -s sqlserver-rm88233 -n AllowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
```
==> Subindo Imagem ACR <==

docker azure login

az login

az acr create --resource-group rg-checkpoint --name epictaskcheckpointacr --sku Basic

az acr login --name epictaskcheckpointacr

docker build -t epic-task .

docker tag epic-task epictaskcheckpointacr.azurecr.io/epic-task:v1

docker push epictaskcheckpointacr.azurecr.io/epic-task:v1

docker rmi epictaskcheckpointacr.azurecr.io/epic-task:v1 -f

docker image rm epic-task

az acr repository list --name epictaskcheckpointacr --output table
```

```

==> Deploy de Imagem do ACR no ACI <==

az login

COPIAR NO PORTAL

az acr login --name epictaskcheckpointacr --username epictaskcheckpointacr --password Wp7RIZGXUNjVZ6MGVcHrJkWCfW+VZG8R

Windows: 

az container create --resource-group rg-checkpoint  --name epictaskcheckpointacr --image epictaskcheckpointacr.azurecr.io/epic-task:v1 --cpu 1 --memory 1 --registry-login-server  epictaskcheckpointacr.azurecr.io --registry-username epictaskcheckpointacr --registry-password Wp7RIZGXUNjVZ6MGVcHrJkWCfW+VZG8R  --ip-address Public  --dns-name-label epictaskcheckpointacr --ports 3000 80 8080

```


```
==> DEPOIS DO DEPLOY <==

Copiar fqdn e IP
    "fqdn": "epictaskcheckpointacr.eastus.azurecontainer.io",
    "ip": "20.246.175.219",

```





## DDL
```
CREATE TABLE tb_role
(
    id   BIGINT AUTO_INCREMENT NOT NULL,
    name VARCHAR(255)          NULL,
    CONSTRAINT pk_tb_role PRIMARY KEY (id)
);

CREATE TABLE task
(
    id            BIGINT AUTO_INCREMENT NOT NULL,
    title         VARCHAR(255)          NULL,
    description VARCHAR(255)          NULL,
    score         INT                   NOT NULL,
    status        INT                   NOT NULL,
    CONSTRAINT pk_task PRIMARY KEY (id)
);

CREATE TABLE tb_user
(
    id       BIGINT AUTO_INCREMENT NOT NULL,
    name     VARCHAR(255)          NULL,
    email    VARCHAR(255)          NULL,
    password VARCHAR(255)          NULL,
    CONSTRAINT pk_tb_user PRIMARY KEY (id)
);

CREATE TABLE tb_user_roles
(
    user_id  BIGINT NOT NULL,
    roles_id BIGINT NOT NULL
);

ALTER TABLE tb_user
    ADD CONSTRAINT uc_tb_user_email UNIQUE (email);

ALTER TABLE tb_user_roles
    ADD CONSTRAINT fk_tbuserol_on_role FOREIGN KEY (roles_id) REFERENCES tb_role (id);

ALTER TABLE tb_user_roles
    ADD CONSTRAINT fk_tbuserol_on_user FOREIGN KEY (user_id) REFERENCES tb_user (id);


```
