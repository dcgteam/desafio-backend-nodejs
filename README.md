
# Desafio Back-end - Node JS

Bem vindo(a)! Obrigado por aceitar o nosso desafio.

# Antes de começar....

- [Keep it simple](https://pt.wikipedia.org/wiki/Princ%C3%ADpio_KISS), entendemos que você possui suas prioridades e nossa proposta com esse desafio é ter uma idéia geral de como você faz seus códigos, toma suas decisões arquiteturais e o seu conhecimento geral sobre os assuntos abordados.
- Seu desafio precisa estar versionado no Github, em um repositório público ou privado. Se o repositório for privado, solicite ao RH a lista de emails que deverá receber o acesso ao seu repositório. Não esqueceça de encaminhar o link do repositório no email de retorno!
- Documentação é primordial e vamos nos guiar por ela ;)
- A aplicação ser fácil de ser executada localmente, então abuse de scripts em Makefiles e o que for necessário. 
- Não tem problema se você não conseguir finalizar tudo! Não deixe de enviar seu desafio por isso!

# O desafio
O desafio consiste em criar uma API REST em Node JS para a loja Star Wars que será consumida por um aplicativo (Android e iOS).
Todos os itens serão colocados em um carrinho do lado do aplicativo e passados para a API para realizar uma transação no e-commerce. A sua API deverá conter os métodos conforme descrito abaixo.

O(A) candidato(a) tem a liberdade de realizar o desafio com as ferramentas e bibliotecas que preferir, desde que a documentação seja clara assim como os processos de instalação e execução local. A comunicação conta!

### Requisitos
- Autenticação nas requisições
- Permitir rodar a aplicação com o Docker e via CLI (npm, yarn, etc..)
- Utilizar banco de dados (de qualquer tipo)
- Implementar testes unitários

### Extras
- Utilizar Typescript
- Utilizar Cache(Redis ou semelhante)
- Implementar uma pipeline de integração contínua para essa API utilizando tecnologias como Azure DevOps, TravisCI, Github Actions ou algum outro de sua preferência.
- Subir a aplicação em algum PaaS como o Heroku ou qualquer outro cloud provider (provavelmente você terá que assinar uma conta free tier ou se aproveitar de limites gratuitos oferecidos por cloud providers).


### POST `/starstore/product`
Esse método deve receber um produto novo e inseri-lo em um banco de dados para ser consumido pela própria API.
```json
{
   "title":"Blusa do Imperio",
   "price":7990,
   "zipcode":"78993-000",
   "seller":"João da Silva",
   "thumbnailHd":"https://cdn.awsli.com.br/600x450/21/21351/produto/3853007/f66e8c63ab.jpg",
   "date":"26/11/2015"
}
```
| Campo       | Tipo   |
|-------------|--------|
| title       | String |
| price       | int    |
| zipcode     | String |
| seller      | String |
| thumbnailHd | String |
| date        | String |


### GET `/starstore/products`
Esse método da API deve retornar o seguinte JSON
```json
[
  {
    "title": "Blusa do Imperio",
    "price": 7990,
    "zipcode": "78993-000",
    "seller": "João da Silva",
    "thumbnailHd": "https://cdn.awsli.com.br/600x450/21/21351/produto/3853007/f66e8c63ab.jpg",
    "date": "26/11/2015"
  },
  {
    "title": "Blusa Han Shot First",
    "price": 7990,
    "zipcode": "13500-110",
    "seller": "Joana",
    "thumbnailHd": "https://cdn.awsli.com.br/1000x1000/21/21351/produto/7234148/55692a941d.jpg",
    "date": "26/11/2015"
  },
  {
    "title": "Sabre de luz",
    "price": 150000,
    "zipcode": "13537-000",
    "seller": "Mario Mota",
    "thumbnailHd": "http://www.obrigadopelospeixes.com/wp-content/uploads/2015/12/kalippe_lightsaber_by_jnetrocks-d4dyzpo1-1024x600.jpg",
    "date": "20/11/2015"
  }
]
```

| Campo       | Tipo   |
|-------------|--------|
| title       | String |
| price       | int    |
| zipcode     | String |
| seller      | String |
| thumbnailHd | String |
| date        | String |


Após o usuário adicionar todos os itens desejados no carrinho, ele finalizará a compra.
Para isso, você precisará fazer o método `buy` na sua API.

### POST `/starstore/buy`
Esse método irá receber os dados da compra, junto com os dados do usuário.
```json
{
   "client_id":"7e655c6e-e8e5-4349-8348-e51e0ff3072e",
   "client_name":"Luke Skywalker",
   "total_to_pay":1236,
   "credit_card":{
      "card_number":"1234123412341234",
      "value":7990,
      "cvv":789,
      "card_holder_name":"Luke Skywalker",
      "exp_date":"12/24"
   }
}
```

+ Transaction

| Campo        | Tipo       |
|--------------|------------|
| client_id    | String     |
| client_name  | String     |
| total_to_pay | int        |
| credit_card  | CreditCard |

+ CreditCard

| Campo            | Tipo   |
|------------------|--------|
| card_number      | String |
| card_holder_name | String |
| value            | int    |
| cvv              | int    |
| exp_date         | String |


### GET `/starstore/history`
Esse método deve retornar todos as compras realizadas na API
```json
[
   {
      "client_id":"7e655c6e-e8e5-4349-8348-e51e0ff3072e",
      "purchase_id":"569c30dc-6bdb-407a-b18b-3794f9b206a8",
      "value":1234,
      "date":"19/08/2016",
      "card_number":"**** **** **** 1234"
   },
   {
      "client_id":"7e655c6e-e8e5-4349-8348-e51e0ff3072e",
      "purchase_id":"569c30dc-6bdb-407a-b18b-3794f9b206a8",
      "value":1234,
      "date":"19/08/2016",
      "card_number":"**** **** **** 1234"
   },
   {
      "client_id":"7e655c6e-e8e5-4349-8348-e51e0ff3072e",
      "purchase_id":"569c30dc-6bdb-407a-b18b-3794f9b206a8",
      "value":1234,
      "date":"19/08/2016",
      "card_number":"**** **** **** 1234"
   }
]
```
| Campo            | Tipo   |
|------------------|--------|
| card_number      | String |
| cliend_id        | String |
| value            | int    |
| date             | String |
| purchase_id      | String |

### GET `/starstore/history/{clientId}`
Esse método deve retornar todos as compras realizadas na API por um cliente específico
```json
[
   {
      "client_id":"7e655c6e-e8e5-4349-8348-e51e0ff3072e",
      "purchase_id":"569c30dc-6bdb-407a-b18b-3794f9b206a8",
      "value":1234,
      "date":"19/08/2016",
      "card_number":"**** **** **** 1234"
   },
   {
      "client_id":"7e655c6e-e8e5-4349-8348-e51e0ff3072e",
      "purchase_id":"569c30dc-6bdb-407a-b18b-3794f9b206a8",
      "value":1234,
      "date":"19/08/2016",
      "card_number":"**** **** **** 1234"
   }
]
```
