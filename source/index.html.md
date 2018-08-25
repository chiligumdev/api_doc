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


**Fonts** caso o template tenha alguma font customizada você deve fazer o upload delas neste endpoint e informar o link 
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

Para ter acesso a nossa API você deve criar uma conta em <a href="http://api.chiligumvideos.com/credentials" target="_blank">nosso sistema</a>.

Após criar a conta você deve entrar em contato conosco para mudarmos o status da conta para enabled.

Em nosso sistema no meu "Credentials" você pode pegar o token de acesso. Ele deve ser passado a cada requisição através dos headers:

`token: seutoken`

Também passe o content type nos headers para:

`Content-Type: multipart/form-data`

<aside class="notice">
Você deve substituir <code>seutoken</code> para o token da sua conta.
</aside>

# Fontes

## Overview

Todas as fontes utilizadas para personalizar o seu vídeo devem ser persistidas através deste endpoint, por aqui será possível adicionar, remover e editar fontes cadastradas disponíveis para a utilização durante o processo de criação de seus vídeos.

## Receber todas as fontes

```ruby
require 'httparty'

headers = {
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

fonts = HTTParty.get("https://api.chiligumvideos.com/api/fonts", headers: headers)
fonts.body
``` 
```shell
curl "https://api.chiligumvideos.com/api/fonts"
  -H "token: seutoken"
```
> O comando acima retorna um JSON com todas as fontes disponíveis persistidas pelo usuário em posse do token informado no header da requisição:

```json
[  
   {  
      "id":1,
      "name":"Fonte do Vídeo 1",
      "url":"https://s3.amazonaws.com/apiteaser/fonts/8ed7d3ab7ff119921316dbb2077211b168803a0d.ttf",
      "font_file_name":"RackMultipart20170830-30777-1p780sx.ttf",
      "font_content_type":"application/font-sfnt",
      "metadata":"Helvetica-Normal",
      "created_at":"2017-08-30T19:13:47.722Z",
      "updated_at":"2017-08-30T19:13:47.722Z"
   },
   {  
      "id":2,
      "name":"Fonte do Vídeo 2",
      "url":"https://s3.amazonaws.com/apiteaser/fonts/3269f186d9cf12440428af94e1c622039b7d679a.ttf",
      "font_file_name":"RackMultipart20170830-21726-15ncv0y.ttf",
      "font_content_type":"application/font-sfnt",
      "metadata":"Kiss Me or Not",
      "created_at":"2017-08-30T14:07:05.499Z",
      "updated_at":"2017-08-30T14:07:05.499Z"
   }
]
```

Este endpoint retorna todas as fontes.

### HTTP Request

`GET https://api.chiligumvideos.com/api/fonts`

<aside class="notice">
Não esqueça de informar o token no header
</aside>

## Receber uma fonte especifica


```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

font = HTTParty.get("https://api.chiligumvideos.com/api/fonts/<ID>", headers: headers)
font.body
```

```shell
curl "https://api.chiligumvideos.com/api/fonts/<ID>"
  -H "token: seutoken"
```

> O comando acima deve retornar uma estrutura JSON com informações sobre o registro enviado na requisição com o seguinte formato:

```json
{  
   "id":21,
   "name":"Fonte extra",
   "url":"https://s3.amazonaws.com/apiteaser/fonts/d1dd44a427a1a3eb34b406b1c85ea56dedee4be1.ttf",
   "font_file_name":"RackMultipart20170830-20056-11ep8pj.ttf",
   "font_content_type":"application/font-sfnt",
   "metadata":"Larger Mime",
   "created_at":"2017-08-30T13:15:23.743Z",
   "updated_at":"2017-08-30T13:15:23.743Z"
}

```

Este endpoint retorna uma fonte através de seu ID.

### HTTP Request

`GET https://api.chiligumvideos.com/api/fonts/<ID>`

### URL Parameters

Parâmetro | Descrição
--------- | -----------
ID | ID da fonte que deseja retornar

## Criar uma Fonte

```shell
curl "https://api.chiligumvideos.com/api/fonts" \
-H "token: seutoken" \   
-F "[font]name=logo" \
-F "[font]font=@/Users/Desktop/minhafonte.ttf"
```

```ruby
require 'rest-client'

headers = { 
  'token' => '85ca42d2c1a7bc690af91f25'
}

params = {
  font: { 
    name: 'Minha Fonte',
    font: File.new('/Users/Desktop/minhafonte.ttf')
  }
}

font = RestClient.post('https://api.chiligumvideos.com/api/fonts', params, headers)
font.body
```

> O comando acima deve retornar uma estrutura JSON com informações sobre a fonte que acaba de ser inserida pela API caso obtenha sucesso:

```json
 {  
    "id":29,
    "name":"Minha Fonte",
    "url":"https://s3.amazonaws.com/apiteaser/fonts/8aee59959d03d261b0ce3d9b1a153d9499cfa511.ttf",
    "font_file_name":"Effra_Std_Bd.ttf",
    "font_content_type":"application/octet-stream",
    "metadata":"Effra Bold",
    "created_at":"2017-09-27T16:50:49.899Z",
    "updated_at":"2017-09-27T16:50:49.899Z"
 }
```

### HTTP Request

`POST https://api.chiligumvideos.com/api/fonts`


### Parametrôs do post

Parâmetro | Descrição
--------- | -----------
name  | nome da fonte
fonte | arquivo do post

## Deletar uma Fonte

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

font = HTTParty.delete("https://api.chiligumvideos.com/api/fonts/<ID>", headers: headers)
font.body
```

```shell
curl -X DELETE \
"https://api.chiligumvideos.com/api/fonts/<ID>" \
-H "token: seutoken"

```

> O comando acima deve retornar uma estrutura JSON com informações sobre o registro deletado, contendo também a informação de msg com valor de "deleted" informando o sucesso da requisição:

```json
{  
   "font":{  
      "id":21,
      "name":"Fonte extra",
      "url":"https://s3.amazonaws.com/apiteaser/fonts/d1dd44a427a1a3eb34b406b1c85ea56dedee4be1.ttf",
      "font_file_name":"RackMultipart20170830-20056-11ep8pj.ttf",
      "font_content_type":"application/font-sfnt",
      "metadata":"Larger Mime",
      "created_at":"2017-08-30T13:15:23.743Z",
      "updated_at":"2017-08-30T13:15:23.743Z"
    },
   "msg":"deleted"
}
```

### HTTP Request

`DELETE https://api.chiligumvideos.com/api/fonts/<ID>`

### Parametrôs do delete

Parâmetro | Descrição
--------- | -----------
ID | ID da fonte a ser deletado


# Assets

## Overview

Todos os assets (imagens e vídeos) utilizados dentro do template ou como input de vídeos devem ser feitos o upload através deste endpoint. No momento não suportamos assets de sites ou sistemas de terceiros.

## Receber todos Assets

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

assets = HTTParty.get("https://api.chiligumvideos.com/api/assets", headers: headers)
assets.body
```

```shell
curl "https://api.chiligumvideos.com/api/assets"
  -H "token: seutoken"
```

> O comando acima retorna um JSON com todos os assets disponíveis persistidos pelo usuário em posse do token informado no header da requisição:


```json
[
  {
    "id":1,
    "name":"Abertura_PreRender.mp4",
    "url":"https://s3.amazonaws.com/teaserapi/assets/b4c067a22d9a272493340c654bd4f2b7802d06e3.mp4",
    "hash_name":"b4c067a22d9a272493340c654bd4f2b7802d06e3.mp4",
    "attachment_file_name":"Abertura_PreRender.mp4",
    "attachment_content_type":"application/octet-stream",
    "attachment_file_size":"728537",
    "created_at":"2017-08-23T21:30:26.713Z",
    "updated_at":"2017-08-23T21:30:26.713Z"
  },
  {
    "id":2,
    "name":"logo",
    "url":"https://s3.amazonaws.com/teaserapi/assets/55780e178d171747c0cb81597262f3c7abbae9b8.png",
    "hash_name":"55780e178d171747c0cb81597262f3c7abbae9b8.png",
    "attachment_file_name":"foto.png",
    "attachment_content_type":"application/octet-stream",
    "attachment_file_size":"32013",
    "created_at":"2017-08-23T21:30:25.398Z",
    "updated_at":"2017-08-23T21:30:25.398Z"
  }
]

```

Este endpoint retorna todos os assets.


### HTTP Request

`GET https://api.chiligumvideos.com/api/assets`


<aside class="notice">
Não esqueça de informar o token no header
</aside>

## Receber um Asset específico

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

asset = HTTParty.get("https://api.chiligumvideos.com/api/assets/<ID>", headers: headers)
asset.body
```

```shell
curl "https://api.chiligumvideos.com/api/assets/<ID>"
  -H "token: seutoken"
```

> O comando acima deve retornar uma estrutura JSON com informações sobre o registro enviado na requisição com o seguinte formato:

```json
  {
    "id":1,
    "name":"Abertura_PreRender.mp4",
    "url":"https://s3.amazonaws.com/teaserapi/assets/b4c067a22d9a272493340c654bd4f2b7802d06e3.mp4",
    "hash_name":"b4c067a22d9a272493340c654bd4f2b7802d06e3.mp4",
    "attachment_file_name":"Abertura_PreRender.mp4",
    "attachment_content_type":"application/octet-stream",
    "attachment_file_size":"728537",
    "created_at":"2017-08-23T21:30:26.713Z",
    "updated_at":"2017-08-23T21:30:26.713Z"
  }
```

Este endpoint retorna um asset através de seu ID.

### HTTP Request

`GET https://api.chiligumvideos.com/api/assets/<ID>`

### URL Parameters

Parâmetro | Descrição
--------- | -----------
ID | ID do asset que deseja retornar


## Criar um Asset

```shell
curl "https://api.chiligumvideos.com/api/assets" \
-H "token: seutoken" \   
-F "[asset]name=logo" \
-F "[asset]attachment=@/Users/Desktop/logo.png"
```

```ruby
require 'rest-client'

headers = { 
  'token' => 'seutoken'
}

params = {
  asset: { 
    name: 'logo',
    attachment: File.new('/Users/Desktop/logo.png')
  }
}

asset = RestClient.post('https://api.chiligumvideos.com/api/assets', params, headers)
asset.body
```

> O comando acima deve retornar uma estrutura JSON com informações sobre o asset que acaba de ser persistido pela API caso obtenha sucesso:

```json
  {
    "id":3,
    "name":"logo",
    "url":"https://s3.amazonaws.com/teaserapi/assets/c8efe50d7b34615ce997a732fdf0da06954bd962.png",
    "hash_name":"c8efe50d7b34615ce997a732fdf0da06954bd962.png",
    "attachment_file_name":"logo.png",
    "attachment_content_type":"application/octet-stream",
    "attachment_file_size":"32013",
    "created_at":"2017-09-12T03:47:08.078Z",
    "updated_at":"2017-09-12T03:47:08.078Z"
  }
```

### HTTP Request

`POST https://api.chiligumvideos.com/api/assets`


### Parametrôs do post

Parâmetro | Descrição
--------- | -----------
name | nome do asset
attachment | arquivo do post


## Deletar um Asset

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

asset = HTTParty.delete("https://api.chiligumvideos.com/api/assets/<ID>", headers: headers)
asset.body
```

```shell
curl -X DELETE \
"https://api.chiligumvideos.com/api/assets/<ID>" \
-H "token: seutoken"

```

> O comando acima deve retornar uma estrutura JSON:

```json
{"msg":"deleted"}
```

### HTTP Request

`DELETE https://api.chiligumvideos.com/api/assets/<ID>`

### Parametrôs do delete

Parâmetro | Descrição
--------- | -----------
ID | ID do asset a ser deletado



# Tracks

## Overview

Todos as faixas de som utilizadas no vídeo devem ser persistidas neste endpoint. Os formatos de mime-type válidos são
**audio/mpeg** e **audio/mp3**. No momento não suportamos faixas de sites ou sistemas de terceiros.

## Receber todas Tracks

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

tracks = HTTParty.get("https://api.chiligumvideos.com/api/tracks", headers: headers)
tracks.body
```

```shell
curl "https://api.chiligumvideos.com/api/tracks"
  -H "token: seutoken"
```

> O comando acima retorna um JSON com todas as tracks disponíveis persistidos pelo usuário em posse do token informado no header da requisição:

```json
[
  {
    "id":1,
    "name":"Track 1",
    "url":"https://s3.amazonaws.com/teaserapi/tracks/3978aadd3b451a3ecbdffc9b3af511f36dce7fc4.mp3",
    "audio_file_name":"track.mp3",
    "audio_file_size":"3221767",
    "created_at":"2017-08-08T07:27:47.237Z",
    "updated_at":"2017-08-08T07:27:47.237Z"
  },
  {
    "id":2,
    "name":"Nome da track",
    "url":"https://s3.amazonaws.com/teaserapi/tracks/9453d9e51c1585cc0f5fa1805eb2736917a3eddd.mp3",
    "audio_file_name":"track.mp3",
    "audio_file_size":"3221767",
    "created_at":"2017-08-08T07:24:55.491Z",
    "updated_at":"2017-08-08T07:24:55.491Z"
  }
]

```

Este endpoint retorna todas as tracks.


### HTTP Request

`GET https://api.chiligumvideos.com/api/tracks`


<aside class="notice">
Não esqueça de informar o token no header
</aside>

## Receber uma Track específica

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

track = HTTParty.get("https://api.chiligumvideos.com/api/tracks/<ID>", headers: headers)
track.body
```

```shell
curl "https://api.chiligumvideos.com/api/tracks/<ID>"
  -H "token: seutoken"
```

> O comando acima deve retornar uma estrutura JSON com informações sobre o registro enviado na requisição com o seguinte formato:

```json
  {
    "id":1,
    "name":"Abertura_PreRender.mp4",
    "url":"https://s3.amazonaws.com/teaserapi/assets/b4c067a22d9a272493340c654bd4f2b7802d06e3.mp4",
    "hash_name":"b4c067a22d9a272493340c654bd4f2b7802d06e3.mp4",
    "attachment_file_name":"Abertura_PreRender.mp4",
    "attachment_content_type":"application/octet-stream",
    "attachment_file_size":"728537",
    "created_at":"2017-08-23T21:30:26.713Z",
    "updated_at":"2017-08-23T21:30:26.713Z"
  }
```

Este endpoint retorna uma track através de seu ID.

### HTTP Request

`GET https://api.chiligumvideos.com/api/tracks/<ID>`

### URL Parameters

Parâmetro | Descrição
--------- | -----------
ID | ID da track que deseja retornar


## Criar uma Track

```shell
curl "https://api.chiligumvideos.com/api/tracks" \
-H "token: seutoken" \   
-F "[track]name=track" \
-F "[track]audio=@/Users/Desktop/logo.png"
```

```ruby
require 'rest-client'

headers = { 
  'token' => 'seutoken'
}

params = {
  track: { 
    name: 'track',
    audio: File.new('/Users/Desktop/track.mp3')
  }
}

track = RestClient.post('https://api.chiligumvideos.com/api/tracks', params, headers)
track.body
```

> O comando acima deve retornar uma estrutura JSON com informações sobre a track que acaba de ser persistida pela API caso obtenha sucesso:

```json
  {
    "id":3,
    "name":"track",
    "url":"https://s3.amazonaws.com/teaserapi/assets/c8efe50d7b34615ce997a732fdf0da06954bd962.mp3",
    "hash_name":"c8efe50d7b34615ce997a732fdf0da06954bd962.png",
    "audio_file_name": "audio_teste.mp3",
    "audio_file_size": "3064832",
    "created_at":"2017-09-12T03:47:08.078Z",
    "updated_at":"2017-09-12T03:47:08.078Z"
  }
```

### HTTP Request

`POST https://api.chiligumvideos.com/api/tracks`


### Parametrôs do post

Parâmetro | Descrição
--------- | -----------
name | nome da track
audio | arquivo do post


## Deletar uma Track

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

track = HTTParty.delete("https://api.chiligumvideos.com/api/tracks/<ID>", headers: headers)
track.body
```

```shell
curl -X DELETE \
"https://api.chiligumvideos.com/api/tracks/<ID>" \
-H "token: seutoken"

```

> O comando acima deve retornar uma estrutura JSON:

```json
{"msg":"deleted"}
```

### HTTP Request

`DELETE https://api.chiligumvideos.com/api/tracks/<ID>`

### Parametrôs do delete

Parâmetro | Descrição
--------- | -----------
ID | ID da track a ser deletada


# Templates

## Overview

 Todos os templates utilizados para a criação de vídeos devem ser persistidos nos próximos endpoints.Todos
 os moldes de vídeos podems ser persistidos nos endpoints abaixo.

## Receber todos Templates

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

templates = HTTParty.get("https://api.chiligumvideos.com/api/templates", headers: headers)
templates.body
```

```shell
curl "https://api.chiligumvideos.com/api/templates"
  -H "token: seutoken"
```

> O comando acima retorna um JSON:

```json
[  
   {  
      "id":42,
      "name":"Second Template",
      "assets":{  
         "Abertura_PreRender.mp4":""
      },
      "config_fields":{  
         "name":"Template_Doencas",
         "duration":"20",
         "variables":{  
            "foto":{  
               "name":"Imagem",
               "type":"img",
               "width":"1600",
               "height":"900",
               "output":"Foto.png"
            },
            "logo":{  
               "name":"Logo",
               "type":"img",
               "width":"1030",
               "height":"470",
               "output":"Logo.png"
            },
            "texto_1":{  
               "font":{  
                  "size":"60",
                  "family":"MyriadPro-Regular"
               },
               "name":"Texto de Abertura",
               "type":"text",
               "color":"#50747B",
               "output":"Texto_1.png"
            },
            "texto_2":{  
               "font":{  
                  "size":"120",
                  "family":"MyriadPro-Semibold"
               },
               "name":"Título",
               "type":"text",
               "color":"#FFFFFF",
               "output":"Texto_2.png"
            },
            "texto_3":{  
               "font":{  
                  "size":"73",
                  "family":"MyriadPro-Semibold"
               },
               "kind":"caption",
               "name":"Descrição",
               "type":"text",
               "align":"West",
               "color":"#FFFFFF",
               "width":"990",
               "height":"265",
               "output":"Texto_3.png"
            },
            "texto_4":{  
               "font":{  
                  "size":"130",
                  "family":"MyriadPro-Bold"
               },
               "kind":"caption",
               "name":"Texto de encerramento",
               "type":"text",
               "align":"center",
               "color":"#FFFFFF",
               "width":"1026",
               "height":"410",
               "output":"Texto_4.png"
            }
         },
         "main_frame":"35",
         "preview_frames":[  
            "35",
            "101",
            "154",
            "238",
            "462",
            "574"
         ],
         "main_composition":"Template_Doencas"
      },
      "fonts":"{}",
      "aepx_file_name":"template.aepx",
      "aepx_content_type":"application/octet-stream",
      "aepx_file_size":"644838",
      "created_at":"2017-09-29T14:26:32.880Z",
      "updated_at":"2017-09-29T14:26:32.880Z"
   },
   {  
      "id":41,
      "name":"First Template",
      "assets":{  
         "Abertura_PreRender.mp4":""
      },
      "config_fields":{  
         "name":"Template_Doencas",
         "duration":"20",
         "variables":{  
            "foto":{  
               "name":"Imagem",
               "type":"img",
               "width":"1600",
               "height":"900",
               "output":"Foto.png"
            },
            "logo":{  
               "name":"Logo",
               "type":"img",
               "width":"1030",
               "height":"470",
               "output":"Logo.png"
            },
            "texto_1":{  
               "font":{  
                  "size":"60",
                  "family":"MyriadPro-Regular"
               },
               "name":"Texto de Abertura",
               "type":"text",
               "color":"#50747B",
               "output":"Texto_1.png"
            },
            "texto_2":{  
               "font":{  
                  "size":"120",
                  "family":"MyriadPro-Semibold"
               },
               "name":"Título",
               "type":"text",
               "color":"#FFFFFF",
               "output":"Texto_2.png"
            },
            "texto_3":{  
               "font":{  
                  "size":"73",
                  "family":"MyriadPro-Semibold"
               },
               "kind":"caption",
               "name":"Descrição",
               "type":"text",
               "align":"West",
               "color":"#FFFFFF",
               "width":"990",
               "height":"265",
               "output":"Texto_3.png"
            },
            "texto_4":{  
               "font":{  
                  "size":"130",
                  "family":"MyriadPro-Bold"
               },
               "kind":"caption",
               "name":"Texto de encerramento",
               "type":"text",
               "align":"center",
               "color":"#FFFFFF",
               "width":"1026",
               "height":"410",
               "output":"Texto_4.png"
            }
         },
         "main_frame":"35",
         "preview_frames":[  
            "35",
            "101",
            "154",
            "238",
            "462",
            "574"
         ],
         "main_composition":"Template_Doencas"
      },
      "fonts":"{}",
      "aepx_file_name":"template.aepx",
      "aepx_content_type":"application/octet-stream",
      "aepx_file_size":"644838",
      "created_at":"2017-09-29T14:26:05.371Z",
      "updated_at":"2017-09-29T14:26:05.371Z"
   }
]

```

Este endpoint retorna todos os templates.


### HTTP Request

`GET https://api.chiligumvideos.com/api/templates`


<aside class="notice">
  Não esqueça de informar o token no header
</aside>

## Receber um Template específico

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

template = HTTParty.get("https://api.chiligumvideos.com/api/templates/<ID>", headers: headers)
template.body
```

```shell
curl "https://api.chiligumvideos.com/api/templates/<ID>"
  -H "token: seutoken"
```

> O comando acima deve retornar uma estrutura de JSON:

```json
 {  
    "id":42,
    "name":"Second Template",
    "assets":{  
       "Abertura_PreRender.mp4":""
    },
    "config_fields":{  
       "name":"Template_Doencas",
       "duration":"20",
       "variables":{  
          "foto":{  
             "name":"Imagem",
             "type":"img",
             "width":"1600",
             "height":"900",
             "output":"Foto.png"
          },
          "logo":{  
             "name":"Logo",
             "type":"img",
             "width":"1030",
             "height":"470",
             "output":"Logo.png"
          },
          "texto_1":{  
             "font":{  
                "size":"60",
                "family":"MyriadPro-Regular"
             },
             "name":"Texto de Abertura",
             "type":"text",
             "color":"#50747B",
             "output":"Texto_1.png"
          },
          "texto_2":{  
             "font":{  
                "size":"120",
                "family":"MyriadPro-Semibold"
             },
             "name":"Título",
             "type":"text",
             "color":"#FFFFFF",
             "output":"Texto_2.png"
          },
          "texto_3":{  
             "font":{  
                "size":"73",
                "family":"MyriadPro-Semibold"
             },
             "kind":"caption",
             "name":"Descrição",
             "type":"text",
             "align":"West",
             "color":"#FFFFFF",
             "width":"990",
             "height":"265",
             "output":"Texto_3.png"
          },
          "texto_4":{  
             "font":{  
                "size":"130",
                "family":"MyriadPro-Bold"
             },
             "kind":"caption",
             "name":"Texto de encerramento",
             "type":"text",
             "align":"center",
             "color":"#FFFFFF",
             "width":"1026",
             "height":"410",
             "output":"Texto_4.png"
          }
       },
       "main_frame":"35",
       "preview_frames":[  
          "35",
          "101",
          "154",
          "238",
          "462",
          "574"
       ],
       "main_composition":"Template_Doencas"
    },
    "fonts":"{}",
    "aepx_file_name":"template.aepx",
    "aepx_content_type":"application/octet-stream",
    "aepx_file_size":"644838",
    "created_at":"2017-09-29T14:26:32.880Z",
    "updated_at":"2017-09-29T14:26:32.880Z"
 }
```

Este endpoint retorna um template através de seu ID.

### HTTP Request

`GET https://api.chiligumvideos.com/api/templates/<ID>`

### URL Parameters

Parâmetro | Descrição
--------- | -----------
ID | ID do template que deseja retornar

## Criar um Template

```shell
curl "https://api.chiligumvideos.com/api/templates" \
-H "token: seutoken" \   
-F "[template]name=nome_do_template" \
-F "[template]assets=@/Users/Desktop/logo.mp4" \
-F "[template]fonts=@Users/Desktop/font.ttf"
-F "[template]config_json=@/Users/Desktop/config_json" \
-F "[template]aepx=@Users/Desktop/template.aepx"
```

```ruby
require 'rest-client'

headers = { 
  'token' => 'seutoken'
}

params = {
  template: { 
    name: 'Second Template',
    assets: {'Abertura_PreRender.mp4': assets},
    fonts: {'font_1': 'path-of_font_file.ttf'}
    config_fields: arquivo_json,
    aepx: File.new('/home/lean/Downloads/template.aepx')
  }
}

template = RestClient.post('https://api.chiligumvideos.com/api/template', params, headers)
```

> O comando acima deve retornar uma estrutura JSON:

```json
  {  
     "id":43,
     "name":"Second Template",
     "assets":{  
        "Abertura_PreRender.mp4":""
     },
     "config_fields":{  
        "name":"Template_Doencas",
        "duration":"20",
        "main_composition":"Template_Doencas",
        "preview_frames":[  
           "35",
           "101",
           "154",
           "238",
           "462",
           "574"
        ],
        "main_frame":"35",
        "variables":{  
           "foto":{  
              "name":"Imagem",
              "type":"img",
              "output":"Foto.png",
              "width":"1600",
              "height":"900"
           },
           "logo":{  
              "name":"Logo",
              "type":"img",
              "output":"Logo.png",
              "width":"1030",
              "height":"470"
           },
           "texto_1":{  
              "name":"Texto de Abertura",
              "type":"text",
              "output":"Texto_1.png",
              "font":{  
                 "family":"MyriadPro-Regular",
                 "size":"60"
              },
              "color":"#50747B"
           },
           "texto_2":{  
              "name":"Título",
              "type":"text",
              "output":"Texto_2.png",
              "font":{  
                 "family":"MyriadPro-Semibold",
                 "size":"120"
              },
              "color":"#FFFFFF"
           },
           "texto_3":{  
              "name":"Descrição",
              "type":"text",
              "output":"Texto_3.png",
              "font":{  
                 "family":"MyriadPro-Semibold",
                 "size":"73"
              },
              "color":"#FFFFFF",
              "kind":"caption",
              "width":"990",
              "height":"265",
              "align":"West"
           },
           "texto_4":{  
              "name":"Texto de encerramento",
              "type":"text",
              "output":"Texto_4.png",
              "font":{  
                 "family":"MyriadPro-Bold",
                 "size":"130"
              },
              "color":"#FFFFFF",
              "kind":"caption",
              "width":"1026",
              "height":"410",
              "align":"center"
           }
        }
     },
     "fonts":{  
        "font_1":"Effra_Std_Rg.ttf"
     },
     "aepx_file_name":"template.aepx",
     "aepx_content_type":"application/octet-stream",
     "aepx_file_size":"644838",
     "created_at":"2017-09-29T17:54:19.932Z",
     "updated_at":"2017-09-29T17:54:19.932Z"
  }
```

### HTTP Request

`POST https://api.chiligumvideos.com/api/templates`


### Parametrôs do post

Parâmetro | Descrição
--------- | -----------
name            | nome do template
assets          | Arquivos de Assets estáticos criados no endpoint de Assets
fonts           | Arquivos de fontes utilizadas no template
config_fields   | Campos dinâmcos do template em formato json
aepx            | Arquivo aepx criado no AfterEffects

## Deletar Template

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

template = HTTParty.delete("https://api.chiligumvideos.com/api/templates/<ID>", headers: headers)
template.body
```

```shell
curl -X DELETE \
"https://api.chiligumvideos.com/api/templates/<ID>" \
-H "token: seutoken"

```

> O comando acima deve retornar uma estrutura JSON:

```json
{"msg":"deleted"}
```

### HTTP Request

`DELETE https://api.chiligumvideos.com/api/templates/<ID>`

### Parametrôs do delete

Parâmetro | Descrição
--------- | -----------
ID | ID do template a ser deletado


# Videos

## Overview

 Todos os vídeos podem ser persistidos através dos próximos endpoints.

## Receber todos os vídeos criados pertencentes ao dono do token informado junto a requisição.

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

videos = HTTParty.get("https://api.chiligumvideos.com/api/videos", headers: headers)
videos.body
```

```shell
curl "https://api.chiligumvideos.com/api/videos"
  -H "token: seutoken"
```

> O comando acima retorna um JSON:

```json
[  
   {  
      "id":2,
      "name":"Video2",
      "url":"https://path_to_server/video_file_name.mp4"
      "thumbnail_url": "https://path_image/uploaded_image_from_thumbnail.png",
      "template_id":42,
      "track_id":18,
      "data":{  
         "logo":"",
         "texto_1":"Texto de Abertura",
         "texto_2":"Titulo",
         "texto_3":"Descricao",
         "texto_4":"Texto de encerramento"
      },
      "created_at":"2017-09-29T19:16:06.206Z",
      "updated_at":"2017-09-29T19:16:06.206Z"
   },
   {  
      "id":1,
      "name":"Video1",
      "url":"https://path_to_server/video_file_name.mp4"
      "thumbnail_url": "https://path_image/uploaded_image_from_thumbnail.png",
      "template_id":42,
      "track_id":18,
      "data":{  
         "logo":"",
         "texto_1":"Texto de Abertura",
         "texto_2":"Titulo",
         "texto_3":"Descricao",
         "texto_4":"Texto de encerramento"
      },
      "created_at":"2017-09-29T19:15:33.821Z",
      "updated_at":"2017-09-29T19:15:33.821Z"
   }
]
```

Este endpoint retorna todos os videos.


### HTTP Request

`GET https://api.chiligumvideos.com/api/videos`


<aside class="notice">
Não esqueça de informar o token no header
</aside>

## Receber um Video específico

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

video = HTTParty.get("https://api.chiligumvideos.com/api/videos/<ID>", headers: headers)
video.body
```

```shell
curl "https://api.chiligumvideos.com/api/videos/<ID>"
  -H "token: seutoken"
```

> O comando acima deve retornar uma estrutura de JSON:

```json
  {  
    "id":1,
    "name":"Video1",
    "url":"https://path_to_server/video_file_name.mp4"
    "thumbnail_url": "https://path_image/uploaded_image_from_thumbnail.png",
    "template_id":42,
    "track_id":18,
    "data":{  
       "logo":"",
       "texto_1":"Texto de Abertura",
       "texto_2":"Titulo",
       "texto_3":"Descricao",
       "texto_4":"Texto de encerramento"
    },
    "created_at":"2017-09-29T19:15:33.821Z",
    "updated_at":"2017-09-29T19:15:33.821Z"
  }
```

Este endpoint retorna um video através de seu ID.

### HTTP Request

`GET https://api.chiligumvideos.com/api/videos/<ID>`

### URL Parameters

Parâmetro | Descrição
--------- | -----------
ID | ID do video que deseja retornar



## Criar um video


```shell
curl "https://api.chiligumvideos.com/api/videos" \
-H "token: seutoken" \   
-F "[video]name=Video1" \
-F "[video]track_id=18", \
-F "[video]template_id=42", \
-F "[video]data=json_fields" \
-F "[video]postback_url=http://domain.com"
```

```ruby
require 'rest-client'

headers = { 
  'token' => 'seutoken'
}

params = {
  video: { 
    name: 'Video1',
    track_id: 18,
    template_id: 42,
    data: {'texto_1': 'Texto de Abertura', 'texto_2': 'Titulo', 'texto_3': 'Descricao', 'texto_4': 'Texto de encerramento', 'logo': @foto },
    postback_url: 'http://domain.com'
 }
}

@video = RestClient.post('https://api.chiligumvideos.com/api/videos/api/videos', params, headers)


```

> O comando acima deve retornar uma estrutura JSON:

```json
  {  
    "id":1,
    "name":"Video1",
    "url":"https://path_to_server/video_file_name.mp4",
    "thumbnail_url": "https://path_image/uploaded_image_from_thumbnail.png",
    "template_id":42,
    "track_id":18,
    "data":{  
      "logo":"",
      "texto_1":"Texto de Abertura",
      "texto_2":"Titulo",
      "texto_3":"Descricao",
      "texto_4":"Texto de encerramento"
    },
    "created_at":"2017-09-29T19:15:33.821Z",
    "updated_at":"2017-09-29T19:15:33.821Z"
  }
```

### HTTP Request

`POST https://api.chiligumvideos.com/api/videos`


### Parametrôs do post

Parâmetro | Descrição
--------- | -----------
name | nome do video
track_id    | id da trilha a ser transmitida junto ao vídeo
template_id | template utilizado para criação do vídeo
data        | dados de preenchimento dos campos disponíveis no template
postback_url | Este campo é opcional caso precise receber uma notificação em seu endpoint 
play_button | (Opcional) Gera a thumbnail do vídeo contendo um botão de play. Recebe true ou false apenas 

<aside class="notice">
  Os parâmetros retornados no postback body { id, url, thumbnail_url, preview_url }.
  Aconselhamos utilizar o programa Ngrok para testar o postback no seu ambiente de desenvolvimento.
</aside>

## Deletar um video

```ruby
require 'httparty'

headers = { 
  'token' => 'seutoken',
  'Content-Type' => 'multipart/form-data'
}

video = HTTParty.delete("https://api.chiligumvideos.com/api/videos/<ID>", headers: headers)
video.body
```

```shell
curl -X DELETE \
"https://api.chiligumvideos.com/api/videos/<ID>" \
-H "token: seutoken"

```

> O comando acima deve retornar uma estrutura JSON:

```json
{"msg":"deleted"}
```

### HTTP Request

`DELETE https://api.chiligumvideos.com/api/videos/<ID>`

### Parametrôs do delete

Parâmetro | Descrição
--------- | -----------
ID | ID do video a ser deletado

# Player de vídeos

## Overview

Todos os vídeos podem ser persistidos no nosso player de vídeos através dos próximos endpoints.

<aside class="notice">
  Não esqueça de informar o token no header, o token usado no player é diferente do token usado na API.
</aside>

Para ter acesso à API do nosso player você deve criar uma conta em <a href="https://player.chiligumvideos.com/" target="_blank">nosso sistema</a> e acessar o menu de Credentials.

Após criar a conta você deve entrar em contato conosco para mudarmos o status da conta para enabled.

No link do vídeo pode ser passado parâmetros opcionais, tais como: redirect_url para o redirecionamento automático após a finalização do vídeo e thumbnail_url para colocar o thumbnail no vídeo.

<aside class="warning">
  Ambos os parâmentros, redirect_url e thumbnail_url, devem ser válidos e a thumbnail_url deve conter apenas a url da imagem não podendo conter quaisquer outros parâmetros.
</aside>

Exemplo prático `https://player.chiligumvideos.com/46f49b30c8?redirect_url=https://www.google.com&thumbnail_url=https://images.pexels.com/photos/126407/pexels-photo-126407.jpeg`

### Parametrôs opcionais

Parâmetro | Exemplo válido | Exemplo inválido
--------- | -------------- | ----------------
redirect_url | https://www.google.com | www.google.com
thumbnail_url | https://www.link_image.com/image.jpg | https://www.link_image.com/image.jpg?opt_param=teste


## Receber todos os vídeos enviados

 Este endpoint retorna todos os vídeos no player pertencentes ao usuário do token

```shell
curl "https://player.chiligumvideos.com/api/videos" 
-H "token: seutoken" \
```

```ruby
require 'rest-client'

headers = {token: 'seutoken'}

videos = RestClient.get('https://player.chiligumvideos.com/api/videos', headers)
videos.body
```

> O comando acima deve retornar uma estrutura JSON:

```json
[
  {
    "id":165,
    "name":"nomevideo",
    "token":"b92b42f9bc",
    "on_queue":false,
    "on_render":false,
    "processed":false,
    "deleted":false,
    "processed_at":null,
    "data_file_name":"teste.mp4",
    "data_content_type":"application/octet-stream",
    "data_file_size":"5146759",
    "data_updated_at":"2018-06-27 16:45:19 +0000",
    "user_id":2,
    "created_at":"2018-06-27T16:45:19.732Z",
    "updated_at":"2018-06-27T16:45:19.732Z",
    "instance_id":null,
    "activated":true,
    "duration":null
  },
  {
    "id":164,
    "name":"nomevideo",
    "token":"d6d791b7a8",
    "on_queue":false,
    "on_render":false,
    "processed":false,
    "deleted":false,
    "processed_at":null,
    "data_file_name":"teste.mp4",
    "data_content_type":"application/octet-stream",
    "data_file_size":"5146759",
    "data_updated_at":"2018-06-27 16:43:28 +0000",
    "user_id":2,
    "created_at":"2018-06-27T16:43:28.026Z",
    "updated_at":"2018-06-27T16:43:28.026Z",
    "instance_id":null,
    "activated":true,
    "duration":null
  }
]
```

### HTTP Request

`GET https://player.chiligumvideos.com/api/videos`

<aside class="notice">
  Não esqueça de informar o token no header
</aside>

## Receber um vídeo específico

 Este endpoint retorna um vídeo através do seu ID

```shell
curl "https://player.chiligumvideos.com/api/videos/<ID>" \
-H "token: seutoken" \
```

```ruby
require 'rest-client'

headers = {token: 'seutoken'}

video = RestClient.get('https://player.chiligumvideos.com/api/videos/<ID>', headers)
video.body
```

> O comando acima deve retornar uma estrutura JSON:

```json

{ 
  "id":162,
  "name":"Samba Rock",
  "token":"dff4f6b549",
  "on_queue":false,
  "on_render":false,
  "processed":true,
  "deleted":false,
  "processed_at":"2018-06-19T18:29:39.762Z",
  "data_file_name":"teste.mp4",
  "data_content_type":"application/mp4",
  "data_file_size":"5146759",
  "data_updated_at":"2018-06-19 17:56:50 +0000",
  "user_id":2,
  "created_at":"2018-06-19T17:56:50.498Z",
  "updated_at":"2018-06-19T18:29:39.764Z",
  "instance_id":null,
  "activated":true,
  "duration":"48.102000"
}
 
```

### HTTP Request

`GET https://player.chiligumvideos.com/api/videos/<ID>`

<aside class="notice">
  Não esqueça de informar o token no header
</aside>

### URL Parameters

Parâmetro | Descrição
--------- | -----------
ID    | ID do vídeo que deseja retornar

## Enviar um vídeo ao player

```shell
curl "https://player.chiligumvideos.com/api/videos" \
-H "token: seutoken" \
-F "[video]name=nomevideo" \
-F "[video]data=@seuvideo.mp4" \
-F "[video]postback_url=http://domain.com"
```

```ruby
require 'rest-client'

headers = {token: 'seutoken'}
params = {
    video: {
          name: 'nomevideo',
          data: File.new('seuvideo.mp4', 'rb'),
          postback_url: 'https://domain.com'
  }
}

video = RestClient.post('https://player.chiligumvideos.com/api/videos', params, headers)
```

> O comando acima deve retornar uma estrutura JSON:

```json
{
  "id":165,
  "name":"nomevideo",
  "token":"b92b42f9bc",
  "on_queue":false,
  "on_render":false,
  "processed":false,
  "deleted":false,
  "processed_at":null,
  "data_file_name":"teste.mp4",
  "data_content_type":"application/octet-stream",
  "data_file_size":"5146759",
  "data_updated_at":"2018-06-27 16:45:19 +0000",
  "user_id":2,
  "created_at":"2018-06-27T16:45:19.732Z",
  "updated_at":"2018-06-27T16:45:19.732Z",
  "instance_id":null,
  "activated":true,
  "duration":null
}
```

### HTTP Request

`POST https://player.chiligumvideos.com/api/videos`

<aside class="notice">
  Não esqueça de informar o token no header
</aside>


### Parametrôs do post

Parâmetro | Descrição
--------- | -----------
name    | nome do video
data    | arquivo do video em .mp4
postback_url | Este campo é opcional caso precise receber uma notificação em seu endpoint 


<aside class="notice">
  Os parâmetros retornados no postback body { id, url }.
  Aconselhamos utilizar o programa Ngrok para testar o postback no seu ambiente de desenvolvimento.
</aside>

## Deletar um vídeo

```shell
curl -X DELETE \
"https://player.chiligumvideos.com/api/videos/<ID>" \
-H "token: seutoken" \
```

```ruby
require 'rest-client'

headers = {token: 'seutoken'}

video = RestClient.delete('https://player.chiligumvideos.com/api/videos/<ID>', headers)
video.body
```

> O comando acima deve retornar uma estrutura JSON:

```json
{"msg":"deleted"}
```

### HTTP Request

`DELETE https://player.chiligumvideos.com/api/videos/<ID>`

<aside class="notice">
  Não esqueça de informar o token no header
</aside>

### URL Parameters

Parâmetro | Descrição
--------- | -----------
ID    | ID do vídeo que deseja retornar


## Consultar Analytics

 Este endpoint, tem como objetivo trazer todos os analytics realizados no último dia, contando a partir do inicio do dia até seu fim.

```shell
curl "https://player.chiligumvideos.com/api/analytics" \
-H "token: seutoken" \
```

```ruby
require 'rest-client'

headers = {token: 'seutoken'}

  video = RestClient.get('https://player.chiligumvideos.com/api/analytics', headers)
  video.body

```

> O comando acima deve retornar uma estrutura JSON:

```json
[
   {
      "id":30,
      "token":"pifsx44orzd",
      "system_name":"Generic Linux",
      "browser_name":"Chrome",
      "browser_version":"68.0.3440.106",
      "screen_resolution":"1366x741",
      "referer":"http://localhost:3000/08d1350e36",
      "ip":"127.0.0.1",
      "current_time":0,
      "mobile":false,
      "desktop":true,
      "full_watched":false,
      "created_at":"2018-08-24T15:03:00.669Z",
      "updated_at":"2018-08-24T15:45:13.436Z",
      "duration":"",
      "ended":false,
      "video_id":72,
      "percentage_watched":null,
      "city_name":null,
      "country_name":null,
      "session":"JKSLFEIYTS1RO",
      "analized":true,
      "region_name":null
   },
   {
      "id":31,
      "token":"f6mhf8t79ls",
      "system_name":"Generic Linux",
      "browser_name":"Chrome",
      "browser_version":"68.0.3440.106",
      "screen_resolution":"1366x741",
      "referer":"http://localhost:3000/08d1350e36",
      "ip":"127.0.0.1",
      "current_time":0,
      "mobile":false,
      "desktop":true,
      "full_watched":false,
      "created_at":"2018-08-24T15:08:54.143Z",
      "updated_at":"2018-08-24T15:44:32.171Z",
      "duration":"",
      "ended":false,
      "video_id":72,
      "percentage_watched":null,
      "city_name":null,
      "country_name":null,
      "session":"JKSLFEIYTS1RO",
      "analized":true,
      "region_name":null
   }
]

```

### HTTP Request

`GET https://player.chiligumvideos.com/api/analytics`

<aside class="notice">
  Não esqueça de informar o token no header
</aside>

## Receber Analytics Por Range

 Este endpoint retorna analytics através do seu intervalo de data de criação, respeitando a regra de que este intervalo deve conter no máximo 30 dias. Caso o intervalo informado não esteja coerente com esta regra, uma mensagem de erro deverá ser retornada ao ususário requerente. Também é preciso que a data inicial "start_date" seja menor que a data final "final_date".

```shell
curl "https://player.chiligumvideos.com/api/analytics_range?start_date=2018-06-13&final_date=2018-06-23" -H "token: tokedeacessoapi"

```

```ruby
require 'rest-client'

headers = { token: 'seutoken', params: { start_date: '2018-06-13', final_date: '2018-06-26' }}

analytics = RestClient.get('https://player.chiligumvideos.com/api/analytics_range', headers)
analytics.body
```

> O comando acima deve retornar uma estrutura JSON:

```json

[
   {
      "id":1,
      "token":null,
      "system_name":null,
      "browser_name":null,
      "browser_version":null,
      "screen_resolution":null,
      "referer":null,
      "ip":"201.221.0.0 ",
      "current_time":null,
      "mobile":false,
      "desktop":false,
      "full_watched":false,
      "created_at":"2018-06-15T16:43:23.963Z",
      "updated_at":"2018-08-24T20:27:51.646Z",
      "duration":null,
      "ended":false,
      "video_id":1,
      "percentage_watched":0.0,
      "city_name":"Montevideo",
      "country_name":"Uruguay",
      "session":null,
      "analized":true,
      "region_name":null
   },
   {
      "id":2,
      "token":"yhpm53zfep",
      "system_name":"Generic Linux",
      "browser_name":"Firefox",
      "browser_version":"61.0",
      "screen_resolution":"1920x1054",
      "referer":"http://localhost:3000/d4a72c4869",
      "ip":"127.0.0.1",
      "current_time":0,
      "mobile":false,
      "desktop":true,
      "full_watched":false,
      "created_at":"2018-06-15T00:00:00.000Z",
      "updated_at":"2018-08-24T20:31:36.163Z",
      "duration":"",
      "ended":false,
      "video_id":16,
      "percentage_watched":null,
      "city_name":null,
      "country_name":null,
      "session":"undefined",
      "analized":true,
      "region_name":null
   }
]

```

### HTTP Request

`GET https://player.chiligumvideos.com/api/analytics_range`

<aside class="notice">
  Não esqueça de informar o token no header junto com os parâmetros de start_date e final_date no formato YYYY/mm/dd como no exemplo acima.
  Lembrando que não podemos exceder o intervalo de 30 dias no filtro e a data inicial não pode ser maior que a data final.
</aside>

### URL Parameters

Parâmetro | Descrição   | Exemplo
--------- | ----------- | --------
start_date    | Data de inicio da criação dos analytics | '2018-06-13'
final_date    | Data limite da criação dos analytics    | '2018-06-14'

