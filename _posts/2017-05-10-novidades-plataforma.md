---
title:  "Atualizações da plataforma - maio/2017 - atualização 2"
date:   2017-05-10 13:00:00 +0000
categories: [Novidades]
tags: [releases]
author: andreb
---

Muitas novidades nesta nova atualização da plataforma [BLiP](https://blip.ai)!

<!--preview-->

### Nuvem de palavras

O dashboard no [portal](https://blip.ai) agora exibe uma **nuvem com as palavras mais enviadas pelos clientes do chatbot**. Esta nuvem é gerada automaticamente para toda palavra recebida com três ou mais caracteres e é útil para entender como os clientes estão interagindo com seu chatbot.

{% include image.html name="word-cloud.png" alt="Nuvem de palavras" %}

### Gráfico de linha

Agora, na análise de dados, é possível criar **gráficos de linha**, além dos de pizza, barras e lista já existentes.

{% include image.html name="data-analysis1.png" alt="Gráfico de linhas" %}

{% include image.html name="data-analysis2.png" alt="Gráfico de linhas" %}

### Extensão contatos

A nova **extensão contatos** permite o gerenciamento da agenda de contatos do chatbot, que pode ser utilizada para armazenamento dos dados dos clientes. É possível salvar informações como nome, endereço, sexo além de outras informações genéricas. Consulte a [documentação](https://portal.blip.ai/#/docs/extensions/contacts) para maiores detalhes.

### Variáveis de mensagens

Agora é possível utilizar **variáveis de contatos** nas mensagens enviadas pelos chatbots o que facilita tanto o envio de mensagens individuais quanto o envio de mensagens em massa. Todas informações da agenda de contatos podem ser utilizadas como variáveis. 

Por exemplo, a seguinte mensagem:

```json
{  
  "id": "1",
  "to": "11121023102013021@messenger.gw.msging.net",
  "type": "text/plain",
  "content": "Olá, ${contact.name}, seja bem vindo ao plano ${contact.extras.plan}!",
  "metadata": {
    "#message.replaceVariables": "true"
  }
}
```

É entregue ao canal de destino da seguinte forma:
```json
{  
  "id": "1",
  "to": "11121023102013021@messenger.gw.msging.net",
  "type": "text/plain",
  "content": "Olá, João da Silva, seja bem vindo ao plano Gold!",
  "metadata": {
    "#message.replaceVariables": "true"
  }
}
```
Consulte [nossa documentação](https://portal.blip.ai/#/docs/extensions/contacts#substitui%C3%A7%C3%A3o-de-vari%C3%A1veis-das-mensagens) para mais informações.

### Tela cheia no BLiP Chat

Agora o **BLiP Chat** abre em tela cheia quando acionado dentro de um website mobile. Desta forma, os clientes mobile podem interagir de maneira mais simples com seu chatbot ou atendentes do seu site.

{% include image.html name="blip-chat.gif" alt="BLiP Chat" %}

Gostou? Deixe seu comentário ou peça ajuda em nosso [fórum](https://forum.blip.ai).
