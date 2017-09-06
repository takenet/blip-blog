---
title:  "Webview e template master - Setembro/2017"
date:   2017-09-06 10:00:00 +0000
categories: [Novidades]
tags: [releases]
author: breno
---

Para melhorar a experiência do usuário no canal do BLiP lançamos o suporte a WebView e também geramos um novo evento no template Master (Análise de dados).

<!--preview-->

## O que mudou ?

A partir de agora é possível abrir uma página Web dentro do proprio canal do BLiP, para usar essa nova funcionalidade basta enviar um [Link da web](https://portal.blip.ai/#/docs/content-types/web-link) com o target self, selfCompact ou selfTall o site é exibido no proprio canal (atenção o link da web precisa ser **HTTPS**), para abrir em uma nova janela utilizar o valor blank (valor padrão caso não seja informado).

Segue os exemplos de como será exibido de acordo com o target.

self:

{% include image.html name="self.png" alt="self" width="70%" %}

selfTall:

{% include image.html name="selfTall.png" alt="self" width="70%" %}

selfCompact:

{% include image.html name="selfCompact.png" alt="self" width="70%" %}

## O que mudou no BOT Master?

No [template Master](http://blog.blip.ai/2017/05/31/novidades-plataforma.html) agora é possível configurar o tempo de sessão que o usuário vai conversar com o bot redirecionado, o valor padrão é de 30 minutos. Isso significa que se o usuário 'Jose' enviar uma mensagem e o Master redirecionar para o BOT B a partir desse momento Jose vai estar conversando com o BOT B por 30 minutos ou até ocorrer outro redirecionamento.

{% include image.html name="timeout.png" alt="self" width="70%" %}

Em análise des dados temos um novo evento de redirecionamento, onde conseguimos visualizar de maneira facíl quais são os bots que estão sendo mais acessados.

{% include image.html name="redirect.png" alt="self" width="70%" %}

Já nos especialistas agora é possível identificar qual o canal de origem de todos os contatos, seja ele um usuário que interagiu diretamente com o seu BOT ou um usuário que veio pelo Master.

{% include image.html name="contatos.png" alt="self" width="70%" %}

Certifique-se de verificar nossa [documentação](https://portal.blip.ai/#/docs/). Para acompanhar as melhorias no serviço, verifique o blog e envie suas idéias de recursos no [fórum](http://forum.blip.ai).

