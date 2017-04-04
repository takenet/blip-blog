---
title:  "Atualizações da plataforma - abril/2017"
date:   2017-04-05 13:00:00 +0000
categories: [Novidades]
tags: [releases]
author: pacheco
---

Mais uma grande **novidade** no [BLiP](https://blip.ai) esta semana 😄. Coloque seu chatbot em uma página web ou em um app mobile.

<!--preview-->

### BLiP Chat

Agora é possível integrar um **chatbot BLiP** em **páginas web** ou em um **aplicativo mobile**.
Para fazer isso é muito simples, basta ir até o [portal](https://blip.ai), publicar seu bot no canal **BLiP Chat** e seguir os passos para a integração.

Veja só como é simples:

#### 1. Clique na opção `Publicações` (no portal do BLiP) e escolha o canal `BLiP Chat`

Ative seu bot no canal clicando na chave (veja figura abaixo).

{% include image.html name="blipChat.png" alt="Canal BLiP Chat" %}

#### 2. Escolha onde deseja publicar seu chatbot: `web` ou em algum `app` (android ou iOS)

{% include image.html name="blipChatWeb.png" alt="Template FAQ" %}

#### 3. *Importante* Defina em quais domínios o chatbot poderá ser integrado

**Por motivos de segurança**, seu chatbot será executado apenas em páginas cujos os domínios estejam cadastrados no portal. Por exemplo, meu chatbot @pachecobot 
é válido apenas nos domínios: 

* localhost (para testes)
* ravpacheco.com (site oficial)

{% include image.html name="blipChatWebSettings.png" alt="Menu persistente" %}

#### 4. Copie o script de integração e cole em sua página.

{% include image.html name="blipChatWebIntegration.png" alt="Menu persistente" %}

São possíveis ainda algumas customizações para personalizar a experiência do usuário com seu chatbot. Para verificar todos os detalhes acesse a documentação do BLiP Chat para as integrações [web](https://github.com/takenet/blip-sdk-web) e mobile [android](https://github.com/takenet/blip-sdk-android) e [iOS](https://github.com/takenet/blip-sdk-ios).

### Observação:

É preciso estar atento para a diferença entre a *API-KEY* do BLiP Chat e o *accessKey* dos chatbots do tipo SDK. Essas chaves são diferentes e servem para propósitos distintos. 

Enquanto o *accessKey* serve para **conectar um bot (do tipo SDK) ao servidor do BLiP**, a *API-KEY* serve apenas para **liberar o acesso de algum bot ao BLiP Chat**.

Ficou com alguma dúvida ? Deixe seu comentário, ou peça ajuda em nosso [fórum](https://forum.blip.ai).