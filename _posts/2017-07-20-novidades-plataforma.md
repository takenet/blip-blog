---
title:  "Atualizações da plataforma - julho/2017 - atualização 2"
date:   2017-07-20 10:00:00 +0000
categories: [Novidades]
tags: [releases]
author: andreb
---

Grandes novidades no *hub* de inteligência artificial do BLiP: Agora é possível criar e refinar seu modelo de IA diretamente no portal!

<!--preview-->

O BLiP suporta a definição de **intenções** e **entidades** diretamente no portal, além de permitir associar **perguntas e respostas** a intenções do usuário. Tudo isso está disponível através do menu *Inteligência artificial* nos detalhes do chatbot.

### Configuração do provedor

Antes de começar, é necessário configurar o seu provedor de inteligência artificial. Atualmente, o BLiP suporta o **IBM Watson Conversation** mas nas próximas semanas serão adicionadas outras opções. Desta forma, você poderá treinar o seu modelo em múltiplos provedores de IA simultaneamente e analisar as mensagens dos usuários do chatbot em todos eles.

{% include image.html name="configuration.png" alt="Configuração de provedor" width="70%" %}

### Intenções

As intenções representam ações ou pedidos que o usuário deseja realizar. Elas mapeiam o que o usuário quer dizer com as ações que seu bot pode executar. 

Para incluir uma nova intenção, acesse o menu *Inteligência artificial* > *Intenções* nos detalhes do chatbot.

{% include image.html name="intention1.png" alt="Intenção" width="70%" %}

Ao cadastrar uma nova intenção, é necessário informar **frases do usuário** que representam exemplos comuns de mensagens que o usuário pode enviar para expressar esta intenção. Também podem ser associadas **respostas** a esta intenção que são retornadas através da API de análise quando a intenção é identificada.

{% include image.html name="intention2.png" alt="Intenção" width="70%" %}

### Entidades

Entidades são informações que são como variáveis das intenções: elas representam o objeto das mesmas.

A inclusão de entidades é feita através do menu *Inteligência artificial* > *Entidades* nos detalhes do chatbot.

{% include image.html name="entity1.png" alt="Entidade" width="70%" %}

Ao incluir uma nova entidade, deve-se informar os valores possíveis da mesma, além de sinônimos para cada um destes valores, para aumentar a assertividade do reconhecimento das mesmas.

{% include image.html name="entity2.png" alt="Entidade" width="70%" %}

### Treinamento e publicação do modelo

Após o cadastro de intenções, frases do usuário e entidades, é necessário **treinar e publicar seu modelo** para utilizá-lo. 

O treinamento e a publicação é realizada através da opção *Publicação* do menu *Inteligência artificial*.

{% include image.html name="publishing1.png" alt="Publicação" width="70%" %}

O primeiro passo é **sempre treinar seu modelo**, para deixá-lo disponível para uso. O treinamento é realizado **em todos os provedores de IA ativos no chatbot** naquele momento. Após o treinamento, deve-se publicar o modelo  para poder utilizá-lo.

{% include image.html name="publishing2.png" alt="Publicação" width="70%" %}

### Analisando as mensagens

Com o modelo publicado, deve-se utilizar a [**extensão de inteligência artificial**](https://portal.blip.ai/#/docs/extensions/artificial-intelligence) para realizar a análise das mensagens dos usuários. Confira nossa [documentação](https://portal.blip.ai/#/docs/extensions/artificial-intelligence) para maiores informações.

### Aprimoramento do modelo

É possível visualizar todas as análises realizadas com seu modelo e as intenções identificadas. Para cada análise, o BLiP permite **sugerir uma intenção diferente** da identificada pelo provedor de IA.

{% include image.html name="enhancement1.png" alt="Publicação" width="70%" %}

Isso permite que o modelo **evolua a partir do feedback** dos usuários. Ao realizar a associação, é gerada uma sugestão que deve ser aprovada pelo  responsável do modelo. 

{% include image.html name="enhancement2.png" alt="Publicação" width="70%" %}

Se aprovada, a frase do usuário é **associada à intenção sugerida** e passa a ser válida para esta intenção no próximo treinamento do modelo.


Ficou com alguma dúvida ? Deixe seu comentário ou peça ajuda em nosso [fórum](https://forum.blip.ai).
