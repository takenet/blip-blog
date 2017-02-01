---
#title:  "4 motivos para fazer seu chatbot no BLiP"
title:  "Como o BLiP pode te ajudar em um hackathon"
date:   2017-02-01 10:00:00 +0000
categories: [Casos de uso]
tags: [Extensões, sdk, vantagens]
author: pacheco
---

O objetivo principal da plataforma BLiP é entregar a desenvolvedores(as) todo o suporte necessário para facilitar e agilizar 
a criação de chatbots. Este artigo apresenta alguns motivos de porquê você deveria utilizar o BLiP durante a construção de 
seu chatbot, dentre eles: **extensões**, **client's**, **publicações em multicanais** e **sdk's web e mobile** 

<!--preview-->

### Extensões

> "Reinventar a roda não é uma opção."

Se você desenvolve software, assim como eu, deve estar acostumado a utilizar bibliotecas, frameworks e ferramentas que aumentam a produtividade de seu projeto. 
**Reinventar a roda não é uma opção**. Pensando nisso, o BLiP possui diversas extensões para as principais necessidades de quem está desenvolvendo um chatbot. Você não precisa sequer de banco de dados para armazenar as 
informações de seus clientes, o BLiP já possui esta extensão.

Atualmente a plataforma conta com as seguintes extensões:

<table>
  <tr>
    <td>Extensão</td>
    <td>Descrição</td>
    <td></td>
  </tr>
  <tr>
    <td>Agendamento (Schedulling)</td>
    <td>Realiza o envio de mensagens, em nome do seu chatbot, para uma data e horário específico</td>
    <td><a href="https://portal.blip.ai/#/docs/extensions/scheduler" target="_blank">Saiba mais</a></td>
  </tr>
  <tr>
    <td>Envio em massa (Broadcast)</td>
    <td>Gerencia listas de usuários de seu chatbot e envia mensagens para todos os usuários de uma lista</td>
    <td><a href="https://portal.blip.ai/#/docs/extensions/broadcast" target="_blank">Saiba mais</a></td>
  </tr>
  <tr>
    <td>Armazenamento (Storage)</td>
    <td>Permite o armazenamento de dados, no formato de documentos JSON, em um local seguro e isolado, acessível apenas pelo seu chatbot</td>
    <td><a href="https://portal.blip.ai/#/docs/extensions/bucket" target="_blank">Saiba mais</a></td>
  </tr>
  <tr>
    <td>Diretório (User Details)</td>
    <td>Consulta informações dos usuários (de acordo com o canal utilizado) de seu chatbot, como nome e outros dados pessoais</td>
    <td><a href="https://portal.blip.ai/#/docs/extensions/directory" target="_blank">Saiba mais</a></td>
  </tr>
  <tr>
    <td>Análise de eventos (Event Track)</td>
    <td>Permite o registro, coleta e análise de eventos em tempo real ocorridos em seu chatbot</td>
    <td><a href="https://portal.blip.ai/#/docs/extensions/event-track" target="_blank">Saiba mais</a></td>
  </tr>
  <tr>
    <td>Histórico de conversas (History)</td>
    <td>Possibilita a consulta das últimas mensagens trocadas entre seu chatbot e um determinado usuário de um canal</td>
    <td><a href="https://portal.blip.ai/#/docs/extensions/threads" target="_blank">Saiba mais</a></td>
  </tr>
</table>

Para consultar a documentação completa de todas as extensões [clique aqui](https://portal.blip.ai/#/docs/extensions)
O artigo ["Entendo as extensões"](http://blog.blip.ai/2016/12/28/entendendo-as-extensoes.html) detalha um pouco mais o assunto.

### *Clients* de integração

> "Por quem desenvolve para quem desenvolve"

O BLiP permite duas formas diferentes para a construção dos chatbots. A primeira delas é através dos *client's* [C#](https://github.com/takenet/messaginghub-client-csharp/) e 
[JavaScript](http://takenet.github.io/messaginghub-client-js/), a principal vantagem deste modelo é entregar uma enorme facilidade durante o desenvolvimento.
A outra forma de realizar a integração é através de um [Webhook](https://blip.ai/portal/#/docs/webhook). 

De forma simplista, um *Webhook* é a exposição de um *endpoint* http em sua aplicação para possibilitar que API's de terceiros faça requisições em seus serviços. 
Nesta estrutura o BLiP realiza requisições em um *endpoint* definido pelo desenvolvedor do chatbot sempre que novas mensagens ou notificações estiverem disponíveis. 
A principal vantagem deste modelo é não estar amarrado a nenhuma linguagem de programção específica.

### Multicanal

> Faça mais gastando menos

Por se tratar de uma plataforma multicanais, os chatbots desenvolvidos através do BLiP podem ser publicadas em vários aplicativos de mensagem como Messenger,
Telegram, SMS, Email, Skype, nas aplicações BLiP e no Whatsapp (assim que sua plataforma esteja aberta oficialmente). 

Assim, toda a implementação e manutenção de seu chatbot acontece uma única vez para todos os canais. Sua equipe pode manter um único código 
em vários canais de forma transparente, atendendo qualquer cliente e gastando menos.

### SDK web e Mobile

> Inclua um chatbot em sua aplicação

Além de disponibilizar os chatbots nos principais canais, em alguns momentos pode ser interessante adicionar o chatbot à sua aplicação. Pensando nisso, o BLiP lançou seu SDK, com versões Web 
e Mobile (Android e iOS).

A documentação de cada SDK estão disponíveis no github:

* [Web](https://github.com/takenet/blip-sdk-web)
* [Android](https://github.com/takenet/blip-sdk-android) 
* [iOS](https://github.com/takenet/blip-sdk-ios)

Com algumas linhas de código seu chatbot pode rodar dentro de seu site ou aplicativo mobile, possibilitando por exemplo, um maior engajamento com os clientes ou facilitando as interações 
durante uma compra. A figura abaixo apresenta um exemplo da utilização do SDK web para a comunicação com o chatbot de um gerente digital.

<figure>
    <img class="alignnone size-full" width="80%" src="http://i.imgur.com/HP4AFkJ.png" alt="blip" />
</figure>

Ficou convencido sobre as vantagens do BLiP ? Acesse agora o [portal](https://portal.blip.ai/#/) e crie seu chatbot gratuitamente. É simples, fácil e rápido.

Se você ainda não está totalmente convencido das vantagens do BLiP veja os artigos: ["Como fazer um chatbot currículo"](http://blog.blip.ai/2016/11/05/resumo-chatbot-webhook.html), 
["Chatbot sem código"](http://blog.blip.ai/2016/11/25/chatbot-sem-codigo.html) e ["Chatbots e a importancia de contexto"](http://blog.blip.ai/2016/12/15/chatbots-e-a-importancia-do-contexto.html) eles apresentam outros exemplos de facilidades da plataforma.

Caso tenha qualquer dúvida, crítica ou sugestão deixe aqui seu comentário. Se prefirir envie um email para 'blip@take.net' ou entre em contato com nosso [chatbot](https://m.me/blipajuda).

