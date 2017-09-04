---
title:  "SDK C# com suporte a .NET Core"
date:   2017-09-04 18:00:00 +0000
categories: [Novidades]
tags: [releases]
author: andreb
---

Agora é possível criar chatbots multiplataforma utilizando a nova versão do **SDK C# no BLiP**!

<!--preview-->

Lançamos na semana passa uma nova versão no **[SDK C# do BLiP](https://portal.blip.ai/#/docs/sdks/csharp)**, que permite a construção de aplicações C# multiplataforma (Windows, Linux e Mac) e que podem ser facilmente hospedados na nuvem, inclusive em [containers Docker](https://www.docker.com/). Isso é possível graças a portabilidade do [.NET Core](https://dot.net/core) no qual o mesmo é baseado.

Consulte [nossa documentação](https://portal.blip.ai/#/docs/sdks/csharp/installing) para saber como começar a utilizá-lo.

### Migrando do antigo SDK

Para realizar a migração de um chatbot que utiliza a versão antiga do SDK (baseada no pacote `Takenet.MessagingHub.Client`) é necessário realizar os seguintes passos:

- Criar um novo projeto utilizando o template do BLiP, utilizando o comando `dotnet new blip-console` (como descrito no tópico [**Instalação**](https://portal.blip.ai/#/docs/sdks/csharp/installing) da documentação).
- Copiar as classes e o arquivo `application.json` para o novo projeto e instalar as demais dependências (exceto do SDK do BLiP, que já está instalada no template).
- Substituir o nome das interfaces e classes antigas para o SDK novo (mapeado abaixo).
- Ajustar as chamadas dos métodos com assinaturas alteradas (mapeado abaixo).

Não é possível reutilizar os projetos antigos pois os mesmos se baseiam na versão antiga do `.csproj` do .NET, que é incompatível com projetos .NET Standard / .NET Core utilizados no novo SDK do BLiP.

### Mapeamento de tipos

| Antigo                                                       | Novo                                       |
|--------------------------------------------------------------|--------------------------------------------|
| `Takenet.MessagingHub.Client.Sender.IMessagingHubSender`     | `Take.Blip.Client.ISender`                 |
| `Takenet.MessagingHub.Client.Listener.IMessageReceiver`      | `Take.Blip.Client.IMessageReceiver`        |
| `Takenet.MessagingHub.Client.Listener.INotificationReceiver` | `Take.Blip.Client.INotificationReceiver`   |
| `Takenet.MessagingHub.Client.Listener.ICommandReceiver`      | `Take.Blip.Client.ICommandReceiver`        |
| `Takenet.MessagingHub.Client.IMessagingHubClient`            | `Take.Blip.Client.IBlipClient`             |
| `Takenet.MessagingHub.Client.MessagingHubClientBuilder`      | `Take.Blip.Client.BlipClientBuilder`       |
| `Takenet.MessagingHub.Client.Extensions.*`                   | `Take.Blip.Client.Extensions.*`            |
| `Takenet.MessagingHub.Client.Listener.*`                     | `Take.Blip.Client.Receivers.*`             |
| `Takenet.MessagingHub.Client.Host.*`                         | `Take.Blip.Client.Activation.*`            |

### Mapeamento de métodos

| Antigo                                                  | Novo                                                                      |
|---------------------------------------------------------|---------------------------------------------------------------------------|
| `IMessagingHubSender.SendCommandAsync(Command)`         | `Take.Blip.Client.ISender.ProcessCommandAsync(Command, CancellationToken)`|
| `IMessagingHubSender.SendCommandResponseAsync(Command)` | `Take.Blip.Client.ISender.SendCommandAsync(Command, CancellationToken)`   |

Observação: Na maior parte dos métodos da nova versão, é obrigatório informar um `cancellationToken` para garantir o cancelamento correto das operações assíncronas e evitar o congelamento do processo. Neste caso, passe sempre o `cancellationToken` recebido nos *receivers*.

### Executando

No template do SDK antigo, o projeto era baseado em uma *Class library*, sendo hosteado pelo utilitário `mhh.exe`, que não existe mais na nova versão. Isso é pelo fato do novo SDK ser multiplataforma e o executável `mhh.exe` ser exclusivo para Windows. No novo template, o chatbot é criado como um `console application`. Apesar disso, é possível instalá-lo como serviço Windows, de forma semelhante ao SDK antigo. Veja mais detalhes na seção [**Hospedagem**]((https://portal.blip.ai/#/docs/sdks/csharp/hosting) da documentação.

Mas caso deseje que seu chatbot seja ainda um *Class library* por qualquer motivo, utilize o template `blip-console` ao criar o projeto e o altere para *Class library*. Para executar o chatbot neste caso, será necessário um outro projeto como host.

Ficou alguma dúvida? Deixe seu comentário ou peça ajuda em [nosso fórum](http://forum.blip.ai).

