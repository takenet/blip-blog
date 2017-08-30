---
title:  "Reformulação Análise de dados - Agosto/2017 - atualização 2"
date:   2017-08-30 10:00:00 +0000
categories: [Novidades]
tags: [releases]
author: pacheco
---

Com o intuito de facilitar ainda mais a análise de dados dos bots, o [BLiP](https://blip.ai) lançou mais uma super novidade. 
A partir de agora, além de criar gráficos customizados por bot, é possível também:

* Categorizar os gráficos em relatórios personalizados;
* Criar múltiplos relatórios (públicos ou privados);
* Criar gráficos do tipo **barra** 😯.

<!--preview-->

## O que mudou ?

A partir de agora, o antigo módulo **Painel** passa a se chamar **Análise de dados**, refletindo ainda mais o propósito da funcionalidade.

{% include image.html name="compAnalyses.png" alt="Comparação entre o módulo de Análise de dados novo e antigo" width="60%" %}

A permissão **Análise de dados** foi substituida pela permissão **Relatórios customizados**. 

{% include image.html name="compPermissions.png" alt="Comparação entre o modo de permissão novo e antigo" width="90%" %}

- Os usuários que tiverem permissão de leitura, poderão **apenas ver** todos os gráficos de todos os relatórios *públicos*.
- Os usuários que tiverem permissão de leitura e escrita, poderão **ver, editar, mudar a ordem e exlcuir** todos os gráficos criados em seus próprios relatórios. Além disso, usuários com permissão de leitura ou escrita podem visualizar relatórios *públicos* de outras pessoas.
- Os usuários que não tiverem a permissão **Relatórios customizados** não poderão ver nem criar relatórios customizados. Entretanto, os mesmos ainda poderão ver os dados de **Visão geral** do bot.

> **Obs.: Todos os gráficos que existiam na aba Análise de dados foram mantidos, mas podem ser excluídos a partir de agora**

Com todas essas mudanças o fluxo de criação de gráficos customizados sofreu algumas alterações:

### Criando um relatório customizado

Qualquer usuário que possua permissão de escrita para os **Relatórios customizados** poderá criar um relatório e adicionar gráficos de acordo com sua necessidade. Para isso, basta clicar no botão *+ Criar relatório*, definir um nome e a visibilidade do seu relatório (conforme imagem abaixo).

{% include image.html name="createReport.png" alt="Criando um relatório" width="70%" %}

Caso a opção **Visível somente para mim** seja selecionada o relatório __não estará__ visível para nenhum integrante da equipe, mesmo que eles possuam permissão de leitura ou escrita. O objetivo desta opção é permitir que os usuários consigam criar análises individuais em qualquer bot.

### Adicionando gráficos em um relatório customizado

Depois de criar um relatório, o usuário pode incluir quantos gráficos achar necessário. Para isto, basta clicar no botão *Adicionar Gráfico*, escolher um formato, definir o nome e a categoria de eventos que servirá como fonte dos dados para o gráfico (conforme imagens abaixo).

Passo 1:

{% include image.html name="createGraph.png" alt="Criando um gráfico em um relatório" width="70%" %}

Passo 2:

{% include image.html name="createGraphStep2.png" alt="Criando um gráfico em um relatório - Passo 2" width="70%" %}

Após definida todas as informações do gráfico o mesmo aparecerá dentro do relatório. Note que ainda é possível reoordenar a ordem dos gráficos em um relatório. Isso facilita bastante na organização das informações. A imagem abaixo apresenta um gráfico de "Barras" como exemplo.

{% include image.html name="viewGraph.png" alt="Criando um gráfico em um relatório - Passo 2" width="70%" %}

Viu como é simples 😎? Acesse o portal e crie, hoje mesmo, seu relatório. **Mas lembre-se, para conseguir criar um gráfico você precisa também _trackear_ os eventos no bot ;)**

Gostou das novidades ❤️? Sentiu falta de alguma coisa? Tem alguma sugestão ou crítica? Por favor, deixe seu comentário ou peça ajuda em nosso [fórum](https://forum.blip.ai).


