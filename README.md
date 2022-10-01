> ## DevOps Tools & Cloud Computing - João Carlos Menk

## Arquitetura 2º Checkpoint: Projeto DimDim
<img align="center" alt="BeLive-HTML" src="https://i.imgur.com/5sSmpRf.png">

### Obs: Esse projeto foi baseado no código e matéria de Digital Business Enablement 

# Cliente - Customer
### POST REGISTER

[POST] - http://localhost:8080/user/customer/register
```
{
	"name": "Everton",
	"address": {
		"street": "Rua das Ruas",
		"district": "Bairro",
		"city": "Cidade",
		"state": "Estado",
		"zipCode": "00000-000"
	},
	"userLogin" : {
		"username" : "cliente@email.com",
		"password" : "colocarSenha"
	},
	"cpf": "000.000.000-00"
}
```

### POST LOGIN

[POST] - http://localhost:8080/user/customer/login
```
{
	"username" : "cliente@email.com",
	"password" : "colocarSenha"
}
```

### GET CUSTOMER

[GET] - http://localhost:8080/user/customer/get

Authorization -> **Type:** Basic Auth

```diff
- (* required)
```
- [x] Username
- [x] Password


### PATCH UPDATE CUSTOMER

[PATCH] - http://localhost:8080/user/customer/update
```
{
	"name": "nomeCliente"
}
```
Authorization -> **Type:** Basic Auth

```diff
- (* required)
```
- [x] Username
- [x] Password

### DELETE CUSTOMER

[DELETE] - http://localhost:8080/user/customer/delete

Authorization -> **Type:** Basic Auth

```diff
- (* required)
```
- [x] Username
- [x] Password


## DDL
```

```
