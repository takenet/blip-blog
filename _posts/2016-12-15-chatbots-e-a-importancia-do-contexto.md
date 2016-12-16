---
title:  "Chatbots e a importância do contexto"
date:   2016-12-15 13:26:00 +0000
categories: [Casos de uso]
tags: [Textc, C#]
author: andreb
---

Imagine você chegando no seu trabalho de manhã e ouvir, de longe, a seguinte conversa:

```
João: Já passou da hora de demitir o treinador!
José: Pois é, mas a diretoria insiste em mantê-lo.
```
Se você não é o João ou o José ou não os conhece, provavelmente você são sabe:

- De qual esporte estão falando
- Qual técnico que estão se referindo

Estas informações fazem parte do **contexto** da conversa. O contexto depende de muitas fatores, como por exemplo da **data, horário e local da conversa**. 

<!--preview--> 

Imagine que agora você sabe que esta conversa aconteceu numa quinta-feira de manhã, em Belo Horizonte, depois da final da Copa do Brasil de 2016 entre Atlético e Grêmio, sendo que o Atlético perdeu o jogo.

Com esta informação, podemos deduzir que a resposta destas perguntas são:

- Futebol
- Marcelo Oliveira, técnico do Atlético

Continuando a conversa:

```
João: Mas quem podemos contratar se o mandarmos embora?
José: Não sei, só sei que não podemos mantê-lo por mais tempo.
```

Se o José fosse seu chefe e se você chegasse alguns segundos depois para ouvir somente este último trecho, poderia até ficar preocupado com seu emprego. Mas como você está *contextualizado*, sabe que estão se referindo ao técnico do time que perdeu ontem. O contexto, portanto, também depende de **informações dinâmicas definidas durante a conversação** e que podem ser alteradas a todo momento.

Conversas naturais possuem contextos bastante complexos e que variam de acordo com a relação entre as pessoas. Pessoas mais próximas precisam trocar menos palavras para se entender, praticamente definindo uma linguagem particular. Já conversas entre pessoas desconhecidas tendem a ser mais formais e padronizadas, como em **conversas de atendimento** - onde *chatbots* podem desempenhar um papel suficientemente bom para substituir humanos em boa parte das situações. E quanto mais **sensível ao contexto** o *bot* for, maior chance da conversa ser produtiva para o cliente.

Ao construir um chatbot, deve se considerar que o usuário poderá tentar se comunicar da maneira que ele conversa com outras pessoas da sua lista de contatos. Por outro lado, o **chatbot deve possuir um propósito claro** e delimitar até onde consegue ir. Não adianta tentar responder *qual o sentido da vida* em um bot da pizzaria, seria inviável a implementação devido a complexidade e provavelmente você não atenderia seu cliente bem no que você se propõe - vender pizzas. Desta forma, o contexto do bot da pizzaria deveria se preocupar apenas com informações como nome do cliente, endereço e último pedido que já seria suficiente para atender seu propósito.

## Utilizando Textc para construir uma conversa contextual

No [artigo anterior](http://blog.blip.ai/2016/10/17/chatbots-com-textc.html), apresentamos o [Textc](https://github.com/takenet/textc-csharp) e como utilizá-lo para reconhecer comandos básicos dos usuários. Uma funcionalidade importante da biblioteca é o contexto, que provê **informações que podem ser utilizadas para complementar a entrada do usuário** e satisfazer sintaxes.

Pensando no bot da pizzaria, imagine que ele devesse suportar um comando com a seguinte estrutura:

```
Mande uma pizza grande sabor Marguerita para à Rua Acme, 1234
```

Precisariamos da seguinte **sintaxe**:

```
:Word?(quero,mande,solicito) :Word?(uma) :Word?(pizza) size:Word(pequena,media,grande,gigante) :Word?(sabor,de) flavor:Word(marguerita,pepperoni,calabreza) :Word?(para) :Word?(à,a,o) address:Text
```

As variáveis que temos são:

- `size`: O tamanho da pizza
- `flavor`: Opção de sabor
- `address`: Endereço para entrega

E o seguinte método para processar o pedido:

```csharp
public Task<string> ConfirmOrderAsync(string size, string flavor, string address, IRequestContext context)
{
    // 1. Cria e armazena o pedido
    var order = new Order
    {
        Size = size,
        Flavor = flavor,
        Address = address
    };

    var orderId = ++_globalOrderId;
    _orderDictionary.Add(orderId, order);

    // 2. Salva o pedido no contexto
    context.SetVariable(nameof(orderId), orderId);

    // 3. Envia o mensagem de confirmação
    var builder = new StringBuilder();
    builder.AppendLine($"Seu pedido ({orderId}):");
    builder.AppendLine($"- Sabor: {flavor}");
    builder.AppendLine($"- Tamanho: {size}");
    builder.AppendLine($"- Endereço para entrega: {address}");
    builder.AppendLine();
    builder.AppendLine("Você confirma ?");

    return Task.FromResult(builder.ToString());
}
```

No código acima, armazenamos o pedido do cliente no contexto e solicitados a confirmação do cliente. Neste caso, o número do pedido fica em uma variável **orderId** e que poderá ser utilizada na sintaxe de confirmação de forma que haverá *match* caso a mesma exista no contexto.

```
:Word(sim) orderId:Long
```

Neste caso, o método para confirmação ficaria assim:

```csharp        
public Task<string> ProcessOrderAsync(long orderId, IRequestContext context)
{
    // 1. Obtem o pedido armazenado
    Order order;
    if (!_orderDictionary.TryGetValue(orderId, out order))
    {
        return Task.FromResult("Ops, não encontrei o pedido solicitado :(");
    }
    _orderDictionary.Remove(orderId);

    // 2. Define no contexto as informações do pedido
    context.SetVariable("size", order.Size);
    context.SetVariable("flavor", order.Flavor);
    context.SetVariable("address", order.Address);
    context.RemoveVariable(nameof(orderId));

    // 3. Confirma seu pedido
    var builder = new StringBuilder();
    builder.AppendLine("Seu pedido foi realizado com sucesso!");
    builder.AppendLine("Ah, salvamos suas preferências para os próximos pedidos :)");

    return Task.FromResult(builder.ToString());
}
```

No segundo passo, armazenamos no contexto as informações do pedido do usuário. Desta forma, caso o cliente envie na próxima mensagem apenas `quero uma pizza`, as variáveis faltantes serão extraídas do contexto, ocorrendo o *match*. Isso é válido mesmo caso o cliente envie uma entrada parcial, como `quero uma pizza calabreza` ou `quero uma pizza media de pepperoni`.

E por último, precisamos tratar a negativa do cliente, que deve cancelar o pedido e limpar os dados contexto. A sintaxe ficaria assim:

```
:Word(nao,não) orderId:Long
```

E a implementação do método:

```csharp        
public Task<string> CancelOrderAsync(long orderId, IRequestContext context)
{
    _orderDictionary.Remove(orderId);
    context.Clear();
    return Task.FromResult("O pedido foi cancelado e suas preferências removidas");
}
```

Executando o exemplo, temos:

```
> Mande uma pizza grande sabor Marguerita para à Rua Acme, 1234
Seu pedido:
- Sabor: marguerita
- Tamanho: grande
- Endereço para entrega: rua acme 1234
Você confirma?

> sim
Seu pedido foi realizado com sucesso!
Ah, salvamos suas preferências para os próximos pedidos :)

> quero uma pizza calabreza
Seu pedido:
- Sabor: calabreza
- Tamanho: grande
- Endereço para entrega: rua acme 1234
Você confirma?

> sim
Seu pedido foi realizado com sucesso!
Ah, salvamos suas preferências para os próximos pedidos :)

> quero uma pizza media de pepperoni
Seu pedido:
- Sabor: pepperoni
- Tamanho: media
- Endereço para entrega: rua acme 1234
Você confirma?

> nao
O pedido foi cancelado e suas preferências removidas

> quero uma pizza marguerita
There's no match for the specified input

```

Apesar de ser um exemplo bastante simples, ilustra como o contexto pode facilitar a navegação por parte do usuário.

O código do Textc e deste e outros exemplos estão <a href="https://github.com/takenet/textc-csharp/tree/master/src/Takenet.Textc.Samples">no Github</a>. 