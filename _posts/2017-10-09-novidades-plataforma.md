---
title:  "Atualizações da plataforma - Outubro/2017"
date:   2017-10-09 11:00:00 +0000
categories: [Novidades]
tags: [releases]
author: andreb
---

Nas últimas semanas o [BLiP](https://blip.ai) recebeu novas funcionalidades para facilitar o desenvolvimento e operação de chatbots. Neste post, iremos detalhar as principais atualizações recebidas.

<!--preview-->

## Transcrição das conversas dos contatos

É possível agora visualizar as conversas com cada cliente na lista de contatos do bot. Para isso, basta encontrar o contato e escolher a opção **Ver mais**.

{% include image.html name="contacts1.png" alt="self" width="70%" %}

As mensagens são exibidas como uma conversa, sendo atualizadas em tempo real.

{% include image.html name="contacts2.png" alt="self" width="70%" %}

## Integração webhook para eventos

O BLiP agora permite a configuração de webhooks personalizados para recebimento de mensagens, eventos (da extensão análise de eventos) e mudanças na agenda de contatos para todos os tipos de chatbot (inclusive modelos). Para isso, basta definir a URL do Webhook através do menu **Integrações**.

{% include image.html name="webhook1.png" alt="self" width="70%" %}

Você pode definir qualquer URL HTTP para receber os eventos de forma assíncrona.

{% include image.html name="webhook2.png" alt="self" width="70%" %}

## Métricas do BLiP em dashboard personalizado

Agora é possível incluir as métricas que o BLiP gera automaticamente para os bots em relatório personalizados que são gerados a partir dos dados da extensão **análise de eventos**.

{% include image.html name="report1.png" alt="self" width="70%" %}

Você pode escolher entre as dimensões **Usuários**, **Mensagens** ou **Eventos personalizados**. Os gráficos são gerados a partir da dimensão escolhida.

{% include image.html name="report2.png" alt="self" width="70%" %}

## Configurações avançadas do bot

Em certos casos, o desenvolvedor precisa ter acesso a configurações internas que o BLiP armazena sobre um chatbot, como o **token de autenticação do Messenger**, por exemplo. Para isso, basta ir na tela de edição do chatbot (o lápis ao lado do nome do mesmo) e escolher a opção *Clique aqui para acessar as configurações avançadas*.

{% include image.html name="configuration1.png" alt="self" width="70%" %}

São listadas todas as configurações que o BLiP armazena sobre o chatbot, sendo possível alterar os valores.

{% include image.html name="configuration2.png" alt="self" width="70%" %}

**Tenha cuidado ao alterar estes valores, pois seu bot poderá deixar de funcionar caso seja feita alguma alteração indevida.**

## Proporção de exibição das mídias

Incluímos a propriedade `aspectRatio` no documento **Link de mídia** que permite a definição do formato de exibição de uma mídia no canal de destino. Esta propriedade é útil especialmente no **Facebook Messenger**, que agora permite a exibição de imagens com proporção 1:1 no *generic template*.

Por exemplo, ao enviar o seguinte documento no BLiP:

```json
{
    "id": "1",
    "to": "1630307207029499@messenger.gw.msging.net",
    "type": "application/vnd.lime.media-link+json",
    "content": {
        "title": "Gato",
        "text": "Segue uma imagem de um gato",
        "type": "image/jpeg",
        "uri": "http://2.bp.blogspot.com/-pATX0YgNSFs/VP-82AQKcuI/AAAAAAAALSU/Vet9e7Qsjjw/s1600/Cat-hd-wallpapers.jpg",
        "aspectRatio": "1:1"
    }
}
```

O mesmo é exibido da seguinte forma no Messenger:

{% include image.html name="aspect-ratio1.png" alt="self" %}

Se for enviada sem a opção `aspectRatio`, a mesma seria exibida assim:

{% include image.html name="aspect-ratio2.png" alt="self" %}

Confira a documentação do tipo [**link de mídia**](http://hmg.portal.blip.ai/#/docs/content-types/media-link) para mais informações.

## Texto rotativo com spintax

Por último, mas não menos importante, implementamos no BLiP o suporte a **texto rotativo** utilizando **spinning syntax** (ou **spintax**) que é uma notação que permite a definição de textos dinâmicos podem alternar de valor a cada vez que são entregues ao cliente. Isso permite a construção de conversas mais fluídas e naturais com seus clientes, de forma a **evitar mensagens repetitivas** que afetam negativamente a experiência do usuário.

Para definir um texto rotativo, basta criar grupos de valores delimitados entre chaves (os caracteres `{` e `}`) com as opções separadas por *pipes* (o caracter `|`).

Um exemplo de spintax é:

`{Oi|Ola}, seja bem-vindo! Como {posso|podemos} te ajudar?`

Esta sintaxe poderá dar saída a qualquer um dos seguintes valores:

- Oi, seja bem-vindo! Como posso te ajudar?
- Olá, seja bem-vindo! Como posso te ajudar?
- Oi, seja bem-vindo! Como podemos te ajudar?
- Olá, seja bem-vindo! Como podemos te ajudar?

Para ativar o processamento nos envios, basta incluir nos medatados da mensagem a propriedade `#message.spinText` com o valor `true`, como no exemplo abaixo:

```json
{
    "id": "1",
    "to": "128271320123982@messenger.gw.msging.net",
    "type": "text/plain",
    "content": "{Oi|Ola}, seja bem-vindo! Como {posso|podemos} te ajudar?",
    "metadata": {
        "#message.spinText": "true"
    }
}
```

Confira nossa documentação para mais informações [sobre **spinning syntax**](http://hmg.portal.blip.ai/#/docs/content-types).

---

Gostou das novidades ❤️? Sentiu falta de alguma coisa? Tem alguma sugestão ou crítica? Por favor, deixe seu comentário ou peça ajuda em nosso [fórum](https://forum.blip.ai).
