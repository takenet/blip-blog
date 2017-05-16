---
title:  "BLiP Chat - Tipos de autenticação"
date:   2017-05-15 15:00:00 +0000
categories: [Novidades]
tags: [releases, autenticação]
author: luizg
---

Com o [BLiP](https://blip.ai), você pode publicar o seu chatbot em vários canais de comunicação, um deles é o **BLiP Chat**, o nosso canal oficial. Você sabia que é possível definir tipos de autenticação para o seu chatbot publicado no  **BLiP Chat**? Confira todas as possibilidades neste post.

<!--preview-->

### Tipos de autenticação no BLiP Chat

Pensando em formas de facilitar a identificação dos seus clientes disponibilizamos 3 tipos de autenticação: **Guest, Login e Dev**. 

#### Guest

A autenticação do tipo Guest pode ser utilizada em um chatbot quando não há necessidade de saber dados específicos do seu usuário,como em um bot para consumo de informações sobre uma banda. Nesse modelo, toda vez que o seu chatbot for acessado o BLiP Chat, ele criará um novo usuário, evitando a necessidade de um cadastro. 

A opção Guest é a padrão do BLiP Chat. Portanto, para adicioná-lo em sua página Web, não é necessário passar nenhuma configuração, como mostra o exemplo abaixo:  

```html
<script>
    (function () {
        window.onload = function () {
            new BlipWebSDK.ChatBuilder()
            .withApiKey('SUA-API-KEY')
            .build();
        }
    })();
</script>
```

A imagem abaixo apresenta a tela inicial do BLiP Chat no modo Guest:

{% include image.html name="guest-auth.png" alt="Autenticação do tipo Guest" %}


#### Login

Algumas vezes é necessário identificar o usuário para que você possa abordá-lo de maneira mais pessoal ou solucionar o problema de forma direcionada, podendo até mesmo entrar em contato por meio do e-mail informado. Nesse caso, o tipo Login irá ajudá-lo, uma vez que solicita ao usuário informações como nome e e-mail.

Para habilitar a autenticação do tipo Login, basta defini-la nas configurações do BLiP Chat. Veja o exemplo abaixo:

```html
<script>
    (function () {
        window.onload = function () {
            var options = {
                config: {
                    authType: BlipWebSDK.AuthType.LOGIN
                }
            };

            new BlipWebSDK.ChatBuilder()
            .withApiKey('SUA-API-KEY')
            .build(options);
          }
    })();
</script>
```

O chatbot *BLiP ajuda* do [portal](https://blip.ai) do BLiP utiliza o tipo Login, veja como é:

{% include image.html name="login-auth.png" alt="Autenticação do tipo Login" %}

#### Dev    

O tipo de autenticaçao Dev pode ser escolhido quando for necessário manter no BLiP Chat a mesma identificação que o cliente utiliza em seu sistema interno. Nesse modo é possível definir os seguintes dados para o seu usuário: *identificador, senha, nome e e-mail*. Veja como configurar no exemplo abaixo:


```html
<script>
    (function () {
        window.onload = function () {
            var options = {
                authType: BlipWebSDK.AuthType.DEV,
                user: { 
                    id: 'IDENTIFICADOR-DO-USUÁRIO', // Obrigatório
                    password: 'SENHA-DO-USUÁRIO', // Obrigatório
                    name: 'NOME-DO-USUÁRIO', // Opcional
                    email: 'EMAIL-DO-USUÁRIO' // Opcional
                },
            };

            new BlipWebSDK.ChatBuilder()
            .withApiKey('SUA-API-KEY')
            .build(options);
          }
    })();
</script>
```

Ao definir este modo, o BLiP Chat mantém um histórico de mensagens. Assim, no próximo contato o usuário terá disponível o contexto da última conversa. 

{% include image.html name="dev-auth.png" alt="Autenticação do tipo Dev" %}

Todos os detalhes para utilização dos modos de autenticação podem ser consultados na documentação do BLiP Chat para as integrações [web](https://github.com/takenet/blip-sdk-web) e mobile [android](https://github.com/takenet/blip-sdk-android) e [iOS](https://github.com/takenet/blip-sdk-ios).


Ainda tem alguma dúvida? Deixe seu comentário ou peça ajuda em nosso [fórum](http://forum.blip.ai).
