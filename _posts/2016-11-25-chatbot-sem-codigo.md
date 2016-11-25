---
title:  "Chatbot sem código"
date:   2016-11-25 10:00:00 +0000
categories: [Casos de uso]
tags: [sdk]
author: andre
---

A plataforma [Blip](https://blip.ai) possui um SDK [C#](https://github.com/takenet/messaginghub-client-csharp/) que facilita bastante a criação de chatbots,
tanto que você pode criar um chatbot sem precisar usar uma única linha de código em C# &#128512;! 

<!--preview-->

Toda a conversa será configurada usando o arquivo JSON utilizado pelo SDK, mas ainda assim será preciso usar o Visual Studio 2015.
Existem 2 *templates* do SDK C# que podem ser utilizados, um para uma [*console application*](https://www.nuget.org/packages/Takenet.MessagingHub.Client.Template/)
e outro para uma [*web application*](https://www.nuget.org/packages/Takenet.MessagingHub.Client.WebTemplate/). Neste exemplo vamos criar uma *web application*.

### Projeto básico
Crie um novo projeto ASP.NET do tipo *ASP.NET Web Application (.NET Framework)*. A versão do *.NET Framework* deve ser 4.6.1 ou superior. 
Na janela para seleção de *templates* escolha a opção *Empty* e desmarque todas as opções na seção *Add folders and core references*, como na figura abaixo.

{% include image.html name="image_0.png" alt="Utilize o template Empty" %}

Abra o *Package Manager Console* e instale o pacote do *template web*:

```
Install-Package Takenet.MessagingHub.Client.WebTemplate
```

Observe que será adicionado um arquivo *application.json*. É nele que faremos todas as configurações. 
Antes de iniciar estas configurações, remova o arquivo `PlainTextMessageReceiver.cs` (é apenas um exemplo, não será usado) e no *application.json*
remova a configuração relativa a esta classe. O arquivo deverá ficar com o conteúdo abaixo:

```
{
  "identifier": "",
  "accessKey": "",
  "messageReceivers": [
  ],
  "settings": {
  },
  "startupType": "Startup",
  "schemaVersion": 2
}
```

### O que são *MessageReceivers*?
É interessante entender como o SDK funciona antes de continuarmos. Toda mensagem que seu chatbot receber deverá passar por um *MessageReceiver*. 
Para isso acontecer, filtros de conteúdos de mensagem devem ser configurados no *application.json*, direcionando a mensagem à uma implementação de um *MessageReceiver*. 

Estes filtros podem ser por tipo de conteúdo (texto puro, imagem, video, etc), pelo conteúdo em si, pelo originador, etc. Veja na [documentação](https://blip.ai/portal/#/docs/sdks/csharp/configuring)
mais detalhes sobre estas e outras opções. 

Toda mensagem que chega no chatbot é avaliada contra os filtros definidos no *application.json*, e todos os filtros que casarem com a mensagem terão seus 
respectivos *MessageReceivers* ativados e executados. 

Porém existe a opção de definir um *MessageReceiver* estático que retorna um conteúdo qualquer, definido diretamente no *application.json*. É este recurso que exploraremos neste artigo.

### O chatbot

Neste exemplo, o chatbot vai iniciar enviando uma mensagem de texto de boas vindas com uma pergunta. A configuração do *MessageReceiver* no *application.json* será:

```
    {
      "response": {
        "mediaType": "text/plain",
        "plainContent": "Olá! Esta é uma demonstração do SDK do Blip. Você é um humano ou um robô?"
      }
    }
``` 

Agora vamos definir uma resposta para o caso de responder que é humano:

```
    {
      "mediaType": "text/plain",
      "content": "humano|umano|gente|pessoa|homem|mulher",
      "response": {
        "mediaType": "text/plain",
        "plainContent": "Eu sou um chatbot, mas tenho vários amigos humanos 😀"
      }
    }
```

Observe que na propriedade `content` foram especificadas várias opções. Esta propriedade, como outras da configuração do *MessageReceiver*, aceita **expressões regulares**.
Esta é uma expressão simples, mas qualquer expressão regular válida em C# pode ser utilizada. 
Com esta expressão, respostas como por exemplo '*Sou humano*' e '*Claro q sou HUMANO!*' casarão com este *MessageReceiver*.

Bacana, mas nossa configuração tem um defeito. Quando você enviar sua resposta o chatbot responderá com a mensagem acima e também enviará a mensagem de boas vindas...
Isto porque o primeiro *MessageReceiver* não tem filtro nenhum definido. 

Uma maneira interessante de corrigir isso é utilizar **estados**. Cada *MessageReceiver* pode definir o estado em que ele será ativado, adicionalmente a todas as configurações relativas ao conteúdo da mensagem.
Para isso deve ser definida a propriedade `state`.
E cada *MessageReceiver* pode também ajustar o estado atual, que será definido ao final da execução, através da propriedade `outState`.

Utilizando este novo recurso, nossos *MessageReceivers* ficarão assim:

```
    {
      "response": {
        "mediaType": "text/plain",
        "plainContent": "Olá! Esta é uma demonstração do SDK do Blip. Você é um humano ou um robô?"
      },
      "state": "default",
      "outState": "question"
    },
    {
      "mediaType": "text/plain",
      "content": "humano|umano|gente|pessoa|homem|mulher",
      "state": "question",
      "response": {
        "mediaType": "text/plain",
        "plainContent": "Eu sou um chatbot, mas tenho vários amigos humanos 😀"
      }
    }
``` 

Veja que o *MessageReceiver* da mensagem de boas vindas tem o `state` configurado para *default*, que é o valor inicial configurado pelo SDK. 
Se um `state` não for adicionado significa que o *MessageReceiver* pode ser aplicado em qualquer estado, o que obviamente não é nossa intenção.
Ainda neste *MessageReceiver* foi configurada o `outState` para *question*. Esta propriedade pode ter qualquer texto, mas é interessante dar um nome 
representativo.

Por fim, adicionamos ao segundo *MessageReceiver* o `state` configurado para *question*. Sem esta configuração, se a mensagem inicial tivesse qualquer
uma das palavras configuradas no `content` este *MessageReceiver* também seria ativado.

Defeito removido, vamos adicionar a segunda reposta:

```
    {
      "mediaType": "text/plain",
      "content": "bot|rob[oô]|chatbot",
      "state": "question",
      "response": {
        "mediaType": "text/plain",
        "plainContent": "Que coincidência, eu também sou! 😉"
      }
    }
``` 

Legal. Porém se nenhuma das 2 respostas casar o chatbot não responderá nada. Vamos incluir uma terceira opção, que só será ativada se nenhuma das demais casar.
Para isso podemos utilizar a propriedade `priority`, com um valor maior do que zero. Observe que o `state` ainda é o *question*.

Vamos também utilizar um [menu de opções](https://blip.ai/portal/#/docs/content-types/select), para facilitar para o usuário escolher a opção correta desta vez.
Observe que a propriedade dentro do `response` deverá ser `jsonContent`, já que se trata de um objeto JSON, em vez de `plainContent`, usado somente para texto puro.

```
    {
      "priority": "1",
      "state": "question",
      "response": {
        "mediaType": "application/vnd.lime.select+json",
        "jsonContent": {
            "text":"Ops, não conheço este ser... Afinal, o que você é?",
            "options":[
              {
                  "text":"Humano"
              },
              {
                  "text":"Robô"
              }
            ]
          }
      }
    }
``` 

O `application.json` final do chatbot ficará como abaixo. Os valores das propriedades `identifier` e `accessKey` devem ser aqueles informados nas configurações
do Painel do Blip. 

```
{
  "identifier": "<seu identificador>",
  "accessKey": "<sua chave de acesso>",
  "messageReceivers": [
    {
      "mediaType": "text/plain",
      "response": {
        "mediaType": "text/plain",
        "plainContent": "Olá! Esta é uma demonstração do SDK do Blip. Você é um humano ou um robô?"
      },
      "state": "default",
      "outState": "question"
    },
    {
      "mediaType": "text/plain",
      "content": "humano|umano|gente|pessoa|homem|mulher",
      "state": "question",
      "response": {
        "mediaType": "text/plain",
        "plainContent": "Eu sou um chatbot, mas tenho vários amigos humanos 😀"
      }
    },
    {
      "mediaType": "text/plain",
      "content": "bot|rob[oô]|chatbot",
      "state": "question",
      "response": {
        "mediaType": "text/plain",
        "plainContent": "Que coincidência, eu também sou! 😉"
      }
    },
    {
      "priority": "1",
      "state": "question",
      "response": {
        "mediaType": "application/vnd.lime.select+json",
        "jsonContent": {
          "text": "Ops, não conheço este ser... Afinal, o que você é?",
          "options": [
            {
              "text": "Humano"
            },
            {
              "text": "Robô"
            }
          ]
        }
      }
    }
  ],
  "settings": {
  },
  "schemaVersion": 2
}
```

Detalhe importante: a implementação atual do `IStateManager` no SDK C#, responsável pelo armazenamento do estado de cada usuário, é feita em memória. 
Isto significa que sempre que reiniciar o processo do chatbot ele *esquecerá* o estado e recomeçará a conversa. 

Uma opção interessante para armazenar permanentemente, que em breve deverá estar disponível no SDK, é utilizar a [extensão de armazenamento](https://blip.ai/portal/#/docs/extensions/bucket) do Blip.
Se quiser contribuir com uma implementação que armazene os estados em algum outro meio permanente, nosso repositório no Github está [aqui](https://github.com/takenet/messaginghub-client-csharp).  