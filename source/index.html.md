---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby

toc_footers:
  - <a href='https://api.chiligumvideos.com/'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introdução

Bem-vindo a API da Chiligum! Você pode usar nossa API para gerar vídeos customizados em massa.

Temos exemplos em Shell e Ruby. Você pode usar qualquer linguagem que tenha suporte a HTTP.

Nossa API é composta por **templates**, **tracks**, **assets**, **fonts** e **vídeos**. Sendo eles:

**Templates** armazena o aepx gerado pelo AfterEffects, assets estáticos, fonts e configurações do template


**Tracks** música do vídeo em mp3. Sendo opcional caso o vídeo não tenha uma faixa de áudio.


**Assets** responsável por armazernar todos vídeos e imagens que são usados no template como assets estáticos e nos vídeos como um logo ou foto enviada por um usuário.


**Fonts** caso o template tenha alguma font customizada você deve fazer o upload delas neste end-point e informar o link 
dentro do end point de templates.


**Vídeos** recebe as informações enviadas a patir de tracks, templates e assets.


<img src='images/introduction.png' class='introduction'>

# Autentificação

> Para autentificar passe o token no header em cada request:

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

request = HTTParty.get("https://api.chiligumvideos.com", headers: headers)
request.body
```


```shell
curl "https://api.chiligumvideos.com/"
  -H "token: seutoken"
```

> Tenha certeza que você mudou `seutoken` com seu token da API.

Para ter acesso a nossa API você deve criar uma conta em [nosso sistema](http://api.chiligumvideos.com/credentials).

Após criar a conta você deve entrar em contato conosco para mudarmos o status da conta para enabled.

Em nosso sistema no meu "Credentials" você pode pegar o token de acesso. Ele deve ser passado a cada requisição através dos headers:

`token: seutoken`

Também passe o content type nos headers para:

`Content-Type: multipart/form-data`

<aside class="notice">
Você deve substituir <code>seutoken</code> para o token da sua conta.
</aside>

# Assets

## Overview

Todos os assets (imagens e vídeos) utilizados dentro do template ou como input de vídeos devem ser feitos o upload através deste end-point. No momento não suportamos assets de sites ou sistemas de terceiros.

## Get All Assets

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

tracks = HTTParty.get("https://api.chiligumvideos.com/api/tracks", headers: headers)
tracks.body
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```


> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint retrieves a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

