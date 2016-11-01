---
title:  "Criando um bot 'currículo' com webhook"
date:   2016-11-05 14:00:00 +0000
categories: [Casos de uso]
tags: [Webhook, c#]
author: pacheco
---

Através da plataforma [blip.ai](https://blip.ai/) você consegue criar chatbots (também conhecidos como 'bots', 'smart contacts', contatos inteligentes ou contatos que entregam serviços) 
nos principais canais de menssageria do mercado, como: [Facebook Messenger](https://www.messenger.com/), [Telegram](https://telegram.org/), [Skype](https://www.skype.com/), SMS, Whatsapp 
(em breve) ou pelo aplicativo [Blip](https://play.google.com/store/apps/details?id=net.take.omni) disponível para android.

O Blip.ai entrega ao desenvolvedor duas formas diferentes para a construção dos bots. A primeira delas é através dos SDK's [C#](https://github.com/takenet/messaginghub-client-csharp/) e 
[JavaScript](http://takenet.github.io/messaginghub-client-js/), neste modelo o desenvolvedor tem total flexibilidade para incluir o chat bot em sua aplicação. 
A outra forma de realizar a integração é através de um [Webhook](https://blip.ai/portal/#/docs/webhook). De forma simplista, um Webhook é a exposição de um endpoint http em sua aplicação 
para possibilitar que API's de terceiros faça requisições em seus serviços. Nesta estrutura o Blip.ai realiza requisições em um endpoint definido pelo desenvolvedor do bot sempre que 
novas mensagens ou notificações estiverem disponíveis. Outra vantagem é não estar amarrado as nenhuma linguagem específica para construir seu chatbot.

O objetivo deste artigo é demonstrar, passo a passo, a criação e publicação de um chatbot **currículo**, através do [blip.ai](https://blip.ai/), utilizando o modelo de integração *Webhook*. 
Para isso será implementado um bot *simples* que responderá à alguns comandos básicos sobre minhas informações profissionais.

<!--preview-->

# Sobre o Bot 'currículo'

Atualmente, quase todo profissional possui um currículo no formato tradicional. Folha A4, tópicos relevantes, preferencialmente com no máximo 2 laudas e algumas seções básicas como: 
*informações gerais*, *formação acadêmica*, *experiência profissional*, *principais habilidades* e *alguma proeficiência em línguas* por exemplo. Existem também versões menos tradicionais 
no formato digital, como portifólios, jogos e vídeos explicativos. Recentemente, com a explosão dos chatbots como nova interface de interação entre pessoas e serviços, surgiram alguns 
trabalhos que utilizam esta nova tendência conversacional para apresentar suas experiências profissionais. Destaco aqui, os trabalhos de 
[Esther Crawford](https://medium.com/the-mission/how-i-turned-my-resume-into-a-bot-and-how-you-can-too-f03847352baa#.3hw9lyi3a) e 
[Caio Calado](https://medium.com/@caio_caladoo/ola-caiobot-meu-linkedin-como-um-chatbot-no-messenger-9db6fa736f70#.4awx67ut0) como referências para este assunto.

Assim, nosso objetivo aqui é representar, a partir de um bot, as informações mais importantes sobre minha carreira profissional. 

Para simplificar vamos reduzir o escopo do bot para tratar apenas os seguintes cenários:

1 - Envio de informações gerais: Nome, idade, telefone, email e site
2 - Envio de informações sobre formação acadêmica
3 - Envio das principais experiências profissionais
4 - Envio de uma lista com as principais habilidades

Embora seja possível, não vamos nos preocupar, inicialmente, com uma interpretação elaborada de linguagem natural. O artigo do André Bires 
([Construíndo um bot assistente virtual utilizando o Textc](http://blog.blip.ai/2016/10/17/chatbots-com-textc.html)) dá algumas dicas de como utilizar a biblioteca Textc para melhorar 
a interpretação de texto de seu bot.

# Mãos a obra

## Criando seu chatbot

Antes de mais nada, precisamos criar um novo contato (chatbot) na plataforma [blip.ai](https://blip.ai/).

1. Basta acessar a plataforma, fazer login e clicar no botão **Criar Contato**
2. Escolha a opção Webhook
3. Preencha as informações básicas

## Criando uma API para receber as requisições do blip

Para este artigo utilizarei uma *web API* desenvolvida em C#. Entretanto tenha em mente que a tecnologia escolhida para construir a API não importa, escolha 
aquela que lhe for mais conveniente. Para ver um exemplo de webhook utilizando uma API escrita em Node.JS veja este [post](http://blog.blip.ai/2016/10/24/criando-um-bot-para-busca-imagens-BING.html).

1. Crie um novo projeto de uma *web API* no VisualStudio

O mínimo que precisamos fazer agora é criar dois endpoints na API, um para receber as mensagens enviadas pelos usuários de seu bot e outra para receber notificações.

2. Crie um Controller para Mensagens, com uma action **Post**

[[código básico das duas actions]]

3. Crie um Controller para Notificações, com uma action **Post**

[[código básico das duas actions]]

## Publicando a API no azure

Mais uma vez essa é uma escolha pessoal. Você pode publicar sua API onde se sentir mais confortável, no Azure, AWS, Heroku ou na infra de sua empresa. A única 

coisa que precisamos é de um endereço externo publico válido.

No meu caso a api foi publicada no endereço abc.azurewebsites.net

## Configuração dos endpoints no portal Blip

1. Vá até o portal blip.ai, selecione seu bot e clique **Configurações** na barra lateral esquerda.

2. Insira os endpoints de sua API nos campos 'Url de Mensagens' e 'Url de Notificações' (por exemplo: abc.azurewebsites.net/messages e abc.azurewebsites.net/notificações no meu caso)

## Implementado as respostas de seu bot

Conforme destacado anteriormente nosso bot não terá uma regra muito complexa. O objetivo aqui é apenas provocar o leitor para mais uma aplicabilidade dos chatbots.

Neste sentido o bot será capaz de interpretar comandos de texto com as seguintes sentenças:

<table>
  <tr>
    <td>Sentenças (palavras aceitas)</td>
    <td>Ações</td>
  </tr>
  <tr>
    <td>info | informação | about</td>
    <td>Envia informações gerais</td>
  </tr>
  <tr>
    <td>formação | formacao | education</td>
    <td>Envia dados sobre formação acadêmica</td>
  </tr>
  <tr>
    <td>experiência | experiencia | experience</td>
    <td>Envia as principais experências profissionais</td>
  </tr>
  <tr>
    <td>habilidade | skills</td>
    <td>Envia uma lista com algumas habilidades</td>
  </tr>  
</table>

Pensando nisso seu bot precisará, logo que receber uma mensagem, identificar se o conteúdo enviado bate com algum dos comandos listados acima.

[[codigo]]

Caso a mensagem enviada possua alguma das sentenças o bot deverá responder o conteúdo específico daquele comando. 

[[codigo]]

Finalmente, se o conteúdo recebido não for compatível com nenhuma das sentenças aceitas o bot responderá uma mensagem padrão explicando ao usuário quais são os comandos aceitos.
Todo conteúdo enviado pelo bot deve seguir o padrão dos tipos aceitos pelo blip.ai. Neste [link](https://blip.ai/portal/#/docs/content-types) é possível encontrar todas as opções disponíveis. 

[[codigo]]

## Publicando e testando seu chatbot

Para testarmos nossa aplicação vou publicá-la no facebook messenger. Para isso vá até o portal clique na opção Publicações do meunu lateral esquerdo e depois 

escolha o canal Facebook. Basta seguir o passo a passo indicado e seu bot já estará disponível.

Conclusão

Todo o código desenvolvido para este artigo está disponível em meu github. Caso tenha gostado sinta-se a vontade para utilizá-lo e criar o seu chatbot currículo. 




Imagem:

{% include image.html name="image_7.png" alt="Mapeamento final" %}

