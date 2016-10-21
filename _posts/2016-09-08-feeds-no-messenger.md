---
title:  "Receba seus feeds no Messenger"
date:   2016-09-08 14:00:00 +0000
categories: webhook zapier
author: andre
---

[Webhook](https://blip.ai/portal/#/docs/webhook) é um modelo do Blip que abre os recursos da plataforma para qualquer linguagem de programação, já que praticamente todas as linguagens modernas possuem algum tipo de biblioteca para envio e recebimento de requisições HTTP.

Porém a explosão atual de API’s HTTP nos permite criar chatbots sem a necessidade de uma única linha de código! Ferramentas como [If This Than That](https://ifttt.com/) e [Zapier](https://zapier.com/) oferecem interfaces intuitivas para combinar API´s de forma fácil.

Para demonstrar o poder desta integração, que tal criar um chatbot que envia as última notícias de um *feed* para você no Facebook Messenger (ou em qualquer um dos canais suportados pelo Blip)?

<!--preview-->

Vamos começar criando um novo *zap* no Zapier (você pode clicar [aqui](https://zapier.com/app/editor) para ir direto para o editor; não esqueça que é preciso de uma conta, que pode ser gratuita mesmo). 

Para a *trigger app*, escolha "RSS by Zapier". 

{% include image.html name="image_0.png" alt="Use trigger 'RSS by Zapier'" %}

Escolha a opção "New item in Feed" (deve ser a única) e clique em “Save + Continue”.

{% include image.html name="image_1.png" alt="Use 'new item in Feed'" %}

No passo seguinte, adicione o endereço do *feed RSS* que lhe interessar (como exemplo usei o endereço do *feed* do *blog* Visual Studio). Pode deixar os demais campos em branco e clique no botão "Continue". 

{% include image.html name="image_2.png" alt="Endereço do feed rss" %}

Em seguida, clique em "Fetch & Contine" para que seja feito um teste no endereço do *feed*. Este passo deve ocorrer sem problemas, então clique no botão “Continue” para configurarmos a ação que será tomada quando uma nova notícia for recuperada do *feed*.

Para a *action app*, escolha "Webhooks by Zapier".

{% include image.html name="image_3.png" alt="Use action 'Webhooks by Zapier'" %}

Selecione a opção de POST para a *action* e clique em "Save + Continue".

{% include image.html name="image_4.png" alt="Use POST" %}

Esta é a tela mais importante deste *zap*: o mapeamento do conteúdo do *feed* para o formato Json de mensagem esperado pelo Blip. No Painel do Blip, escolha o contato que será usado para envio das mensagens e entre na tela de Configurações do Webhook. 

{% include image.html name="image_5.png" alt="Mapeamento do feed" %}

Copie a "URL para enviar mensagens" do Blip para a “URL” (exigida) pelo Zapier. O “Payload Type” deverá ser json. 

{% include image.html name="image_6.png" alt="Configurações do Blip" %}

Para a configuração do "Data" vamos montar um envelope de uma mensagem com um link web, conforme a documentação do Blip disponível [aqui](https://blip.ai/portal/#/docs/content-types/web-link):

<table>
  <tr>
    <td>Propriedade (Json)</td>
    <td>Valor</td>
  </tr>
  <tr>
    <td>id</td>
    <td>Guid ou Id (vindo do feed, dependerá do formato do seu feed)</td>
  </tr>
  <tr>
    <td>to</td>
    <td>Endereço do contato que receberá as notícias do feed. Este endereço depende de cada canal.</td>
  </tr>
  <tr>
    <td>type</td>
    <td>Fixo, de acordo com a documentação</td>
  </tr>
  <tr>
    <td>content__uri</td>
    <td>Link (vindo do feed)</td>
  </tr>
  <tr>
    <td>content__text</td>
    <td>Title (vindo do feed)</td>
  </tr>
</table>


Observe que a propriedade content é um outro objeto Json. Para informar as propriedades deste objeto utilizamos a sintaxe especial do Zapier com "__" (2 *undercores*) como separador.
Veja como deverá ficar:

{% include image.html name="image_7.png" alt="Mapeamento final" %}

Por fim, ao final do formulário do Zapier, em "Headers", adicione o cabeçalho Authorization fornecido na página de Configurações do Webhook do seu contato. Observe que ele será dividido entre os 2 campos do formulário.

{% include image.html name="image_8.png" alt="Authorization header" %}

Cliquem agora no botão "Continue". A página seguinte deverá produzir um conteúdo de teste a partir de uma das entradas do *feed*. Clique no botão “Create & Continue” e, se tudo ficou corretamente configurado, você deverá receber uma mensagem no canal que você configurou com o texto da notícia do *feed*. 

Para que o feed seja monitorado e as novas notícias sejam enviadas automaticamente, ligue este novo zap clicando no botão do canto superior direito do Zapier, para que fique na posição "ON". Clique então no botão “Finish”, e aproveite as novidades do seu *feed* favorito ;)

Você pode utilizar outros recursos do Zapier para tornar seu contato ainda mais interessante, como por exemplo criar um *[feed único que agregue todos os seus feeds favoritos](https://zapier.com/blog/make-your-own-rss-superfeed/), e utilizar o endereço deste *feed* agregado, gerado pelo Zapier, neste zap. Ou usar como *trigger app* o Webhook do Zapier, recebendo notícias de serviços especializados de *feed* como [Feedly](https://developer.feedly.com/) ou [SuperFeedr](https://superfeedr.com/subscriber).

