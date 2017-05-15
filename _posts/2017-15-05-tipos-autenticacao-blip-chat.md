---
title:  "Atualizações da plataforma - maio/2017 - atualização 3"
date:   2017-05-12 13:00:00 +0000
categories: [Novidades]
tags: [releases]
author: luizg
---

Você sabia que é possível definir tipos de autenticação para o seu chatbot publicado no canal oficial do [BLiP](https://blip.ai)?

<!--preview-->

### Tipos de autenticação no BLiP Chat

Pensando em formas de facilitar a identificação dos seus clientes disponibilizamos 3 tipos de autenticação: **Guest, Login e Dev**. 

#### Guest

A autenticação do tipo Guest pode ser utilizada em um chatbot quando não há necessidade de saber dados específicos do seu usuário, por exemplo em um bot para consumo de informações sobre uma banda. Nesse modelo, toda vez que o seu chatbot for acessado o BLiP Chat criará um novo usuário, evitando a necessidade de um cadastro. 

Ao adicionar o BLiP Chat em sua página Web ou aplicativo móvel, o tipo Guests será sua opção padrão. A imagem abaixo apresenta a tela inicial do BLiP Chat no modo Guest:

{% include image.html name="guest-auth.png" alt="Autenticação do tipo Guest" %}


#### Login

Algumas vezes é necessário identificar o usuário para que você possa abordá-lo de maneira mais pessoal ou solucionar o problema de forma direcionada, podendo, até mesmo entrar em contato por meio do e-mail informado. Nesse caso, o tipo Login irá ajudá-lo, uma vez que solicita ao usuário informações de nome e e-mail.

Para habilitar a autenticação do tipo Login basta defini-la nas configurações do BLiP Chat. Veja o exemplo abaixo:

```javascript
var options = {
    config: {
        authType: BlipWebSDK.AuthType.LOGIN
    }
};

new BlipWebSDK.ChatBuilder()
  .withApiKey('PUT-YOUR-API-KEY-HERE')
  .build(options);
```

O chatbot *BLiP ajuda* do portal do BLiP utiliza o tipo Login, veja como é:

{% include image.html name="login-auth.png" alt="Autenticação do tipo Login" %}

#### Dev    

O tipo de autenticaçao Dev pode ser escolhido quando for necessário manter no BLiP Chat a mesma identificação que o cliente utiliza em seu sistema interno. Nesse modo é possível definir os seguintes dados para o seu usuário: *identificador, senha, nome e e-mail*. Veja como configurar no exemplo abaixo:

```javascript
var options = {
    authType: BlipWebSDK.AuthType.DEV,
    user: { 
        id: 'USER-IDENTIFIER', // Required
        password: 'USER-PASSWORD', // Required
        name: 'USER-NAME', // Optional
        email: 'USER-EMAIL' // Optional
    },
};

new BlipWebSDK.ChatBuilder()
  .withApiKey('PUT-YOUR-API-KEY-HERE')
  .build(options);
```

Ao definir este modo o BLiP Chat mantém um histórico de mensagens, assim no próximo contato o usuário terá disponível o contexto da última conversa. 

{% include image.html name="dev-auth.png" alt="Autenticação do tipo Dev" %}

Todos os detalhes para utilização dos modos de autenticação podem ser consultados na documentação do BLiP Chat para as integrações [web](https://github.com/takenet/blip-sdk-web) e mobile [android](https://github.com/takenet/blip-sdk-android) e [iOS](https://github.com/takenet/blip-sdk-ios).


Ainda tem alguma dúvida? Deixe seu comentário ou peça ajuda em nosso [fórum](https://forum.blip.ai).
