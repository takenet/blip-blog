---
title:  "Atualizações da plataforma - maio/2017 - atualização 3"
date:   2017-05-31 13:00:00 +0000
categories: [Novidades]
tags: [releases]
author: viniciush
---

Confira as novidades nesta nova atualização da plataforma [BLiP](https://blip.ai)!
Vamos falar do template Master, lista de contatos e imagens no BLiP Chat 

<!--preview-->

### Template de Bot Master

Agora o BLiP conta com uma funcionalidade bem interessante que viabiliza um bot transferir a conversa ativa para outros bots.

Quando o usuário cria um bot utilizando o template Master ele define um conjunto de bots, formado por um bot principal e bots secundários, que trabalharão juntos para responder as mensagens dos clientes. Na prática, o bot Master recebe todas as mensagens e as redireciona para o bot principal ou para os bots secundários. A qualquer momento é possível solicitar ao bot Master a troca do contexto do usuário para qualquer um dos subbots (principal ou secundários).

{% include image.html name="templateMaster.png" alt="Template Master" width="100%" %}

Para informações sobre como configurar e utilizar o modelo Master, [consulte nossa documentação](https://portal.blip.ai/#/docs/templates/master).

### Lista de contatos

No portal do BLiP agora você já pode ver a agenda de contatos do bot, basta acessar o painel selecionar o bot e clicar no menu esquerdo no item Contatos.

{% include image.html name="contatos1.png" alt="Contatos 01" width="100%" %}

{% include image.html name="contatos2.png" alt="Contatos 02" width="100%" %}

### BLiP Chat

Agora no BLiP chat, quando o Bot envia uma imagem e o cliente clica, ela aparece em *full screen*. Para obter essa nova funcionalidade basta atualizar o BLiP Chat para a versão mais nova ( Web = 0.1.22, Android = 0.0.24, iOS = 0.0.28 )

Exemplo:
```html
<script src="https://unpkg.com/blip-chat-web@0.1.19" type="text/javascript"></script>
```

{% include image.html name="full1.png" alt="Image full 1" width="100%" %}

{% include image.html name="full2.png" alt="Image full 2" width="100%" %}

Gostou? Deixe seu comentário ou peça ajuda em nosso [fórum](https://forum.blip.ai).
