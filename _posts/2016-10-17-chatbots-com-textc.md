---
title:  "Construíndo um bot assistente virtual utilizando o Textc"
date:   2016-10-17 15:04:47 +0000
categories: textc c#
author: andreb
---

Os bots de mensagem - ou **chatbots** - são programas que permitem a interação através de mensagens para oferecer algum tipo de serviço. Apesar de não ser uma tecnologia nova, recentemente vem ganhando destaque nas notícias desde que o Facebook, Microsoft e outras empresas anunciaram investimentos em plataformas para construção deste tipo de aplicação.

<!--preview--> 

Um dos principais argumentos para o uso dos bots é que o modelo de **distribuição de serviços através de aplicativos móveis está saturado**. Existe uma resistência por parte dos usuários de instalar novos aplicativos para uso de serviços simples. Por exemplo, imagine que se cada pizzaria de sua cidade oferecesse um aplicativo para realizar o pedido de pizza, você precisaria ter aplicativos diferentes para pedir pizza em lugares variados. Mas e se você não precisasse instalar nenhum aplicativo novo e **utilizar os próprios aplicativos de mensagem que você (provavelmente) já possui para este fim**? É esta oportunidade que o Facebook, Microsoft, Google, Slack e outras grandes empresas estão tentando explorar.

<img class="alignnone size-full wp-image-197" src="https://andrebires.files.wordpress.com/2016/10/bots1.png" alt="bots1" width="864" height="519" />

Oferecer uma interface amigável aos usuários de bots é um grande desafio, já que a forma de entrada padrão disponível é o texto - uma interface sem estrutura. Afinal, **não há garantia do que o usuário vai escrever**, nem da forma, gramática, sintaxe, etc. Existem soluções para tentar contornar esta limitação - como o uso de botões nos canais de mensagem (como Telegram e Messenger) que conduzem a navegação do usuário ou inteligência artificial para tentar "adivinhar" o que o usuário está querendo dizer. Cada uma tem seus prós e contras e neste artigo iremos demonstrar como fazer isso utilizando a **biblioteca de processamento de linguagem natural [Textc](https://github.com/takenet/textc-csharp)**. A biblioteca está disponível para C# e pode ser instalada através do [Nuget](https://www.nuget.org/packages/Takenet.Textc).

De maneira resumida, a **Textc permite definir de sintaxes de texto e associá-las a chamadas de métodos de uma classe**. Uma sintaxe define uma estrutura de texto, com tokens e seus tipos, sendo cada token mapeado a parâmetros de um método. Para ilustrar iremos construir um **assistente virtual que permite o armazenamento de lembretes**, de forma semelhante ao que o Google Now oferece.

## Desenhando a conversa

O primeiro passo é enumerar as diferentes formas que o usuário poderá interagir com seu bot. Alguns exemplos:

1. Lembrar de ir ao médico
2. Lembre me amanhã de pagar a conta de luz
3. Me lembre de fazer compras hoje a tarde

São estruturas de texto diferentes mas que possuem informações parecidas:
- O **comando** para adicionar um lembrete: *Lembrar de, Lembre me, Me lembre de*
- O **texto** do lembrete: *ir ao médico, pagar a conta de luz, fazer compras*
- A **data** do lembrete: *amanhã, hoje*
- A **hora** do lembrete: *a tarde*

No Textc, estas informações são mapeadas em um ou mais **tokens**, sendo que o conjunto de tokens em uma sentença é chamado de **sintaxe**.

Isolando as informações presentes nos exemplos acima, temos:
1. (lembrar de) (ir ao médico)
2. (Lembre me) (amanhã de) (pagar a conta de luz)
3. (Me lembre de) (fazer compras) (hoje) (a tarde)

Para atendermos a estas sintaxes, precisaremos de implementar em nosso calendário **três comandos** diferentes:
1. Adicionar novo lembrete
2. Adicionar novo lembrete para uma data
3. Adicionar novo lembrete para um data e uma hora

Cada comando deverá ser **mapeado a um método diferente** em uma classe, como veremos mais a frente. Em nosso exemplo, teremos apenas uma sintaxe associada por comando, mas é possível ter várias sintaxes diferentes para cada um.

Para representarmos as sintaxes e seus tokens, utilizaremos a CSDL - *Command Syntax Definition Language* - uma notação simples oferecida pela biblioteca. Uma declaração CSDL é constituída de uma ou mais definições de tokens, sendo cada uma representada da seguinte forma:

```
name:Type(initializer)
```

Onde:
- **name** - O nome do token que será extraido da entrada. Este valor pode ser utilizado no mapeamento com os parâmetros do método de uma classe. Opcional.
- **type** - O tipo do token no texto. A biblioteca define alguns tipos como `Word` (uma palavra), `Text` (uma ou mais palavras) e `Integer` (número inteiro). Obrigatório.
- **initializer** - Valor de inicialização, sendo utilizado para limitar os valores válidos para o tipo. Por exemplo, no tipo `Word`, determina quais são as palavras válidas para serem consideradas na entrada do usuário. Opcional.

Sendo assim, podemos representar a primeira sintaxe da seguinte forma:

```
:Word(lembrar) :Word?(de) reminder:Text
```

**Não é necessário nomear tokens que não carreguem informações relevantes** para o processamento do comando, como os dois primeiros desta sintaxe. O importante aqui é só o valor de *reminder*, que é o texto do lembrete. Além disso, alguns tokens podem ser marcados como **opcionais** em uma sintaxe, bastando incluir um ponto-de-interrogação depois da declaração do tipo - como fizemos na preposição "*de"* acima. Neste caso, a sintaxe é válida para entradas como *lembrar de ir ao médico* ou *lembrar médico*.

A sintaxe do segundo comando é semelhante a primeira, com uma informação adicional - a data do lembrete. Ela também inclui outros tokens que não estão presentes na primeira mas que apenas constituem a estrutura do texto. Representando-a com CSDL, temos:

```
:Word(lembre) :Word?(me) date:Word?(hoje,amanha,eventualmente) :Word?(de) reminder:Text
```

Por fim, precisamos configurar a terceira sintaxe, que a princípio parece simples:

```
:Word?(me) :Word(lembre) :Word(de) reminder:Text date:Word?(hoje,amanha,eventualmente) :Word?(a) time:Word?(manha,tarde,noite)
```

Só que existe uma pegadinha: por padrão, o processamento ocorre da **esquerda para a direita** e quando alcançado, o token **reminder** do tipo **Text** irá capturar todo o restante da entrada do usuário, e nunca teríamos *match* dos demais tokens à direita do mesmo (*date* e *time*). Isso acontece porque o tipo de token **Text** é **guloso** ou seja, consome todo o restante da entrada do usuário. Por este motivo, ele deve ser o último token a ser processado em uma sintaxe. Para isso, podemos **alterar a direção de parse** em qualquer ponto da sintaxe ao incluirmos o modificador **~** (til) após o tipo de um token.

Como precisamos que o token **reminder** seja o último a ser processado, a direção de parse deve mudar após o processamento do token imediatamente a esquerda deste - no caso a palavra *de*. Neste caso, teríamos:

```
:Word?(me) :Word(lembre) :Word~(de) reminder:Text date:Word?(hoje,amanha,eventualmente) :Word?(a) time:Word?(manha,tarde,noite)
```

Assim, logo após o *match* da palavra *de*, o parse continuará a partir do final da sintaxe, buscando no final da entrada o valor do token *time*.
<h2>Fazendo funcionar</h2>
Para atendermos aos comandos definidos acima, criaremos uma classe `Calendar` com três métodos - uma para cada comando - como a seguir:

```csharp
public class Calendar
{
    public Task AddReminderAsync(string reminder)
        => AddReminderForDateAsync(reminder, "eventualmente");

    public Task AddReminderForDateAsync(string reminder, string date)
        => AddReminderForDateAndTimeAsync(reminder, date, "manhã");

    public async Task AddReminderForDateAndTimeAsync(string reminder, string date, string time)
    {
        // TODO: Store the reminder for the specified date/time
        return $"O lembrete '{reminder}' foi adicionado para {date} no período da {time}";
    }
}
```

Por último, precisamos realizar o *bind* das sintaxes com os comandos, que pode ser feito da seguinte forma:

```csharp
// Initializamos e realizamos o parse das sintaxes
var syntax1 = CsdlParser.Parse(
    ":Word(lembrar) :Word?(de) reminder:Text");
var syntax2 = CsdlParser.Parse(
    ":Word(lembre) :Word?(me) date:Word?(hoje,amanha,eventualmente) :Word?(de) reminder:Text");
var syntax3 = CsdlParser.Parse(
    ":Word?(me) :Word(lembre) :Word~(de) reminder:Text date:Word?(hoje,amanha,eventualmente) :Word?(a) time:Word?(manha,tarde,noite)");

// Incluimos um OutputProcessor para dar saída à resposta dos métodos no Console
var addReminderOutputProcessor = new DelegateOutputProcessor(
    (text, context) => Console.WriteLine(text));

// Instanciamos a nossa classe
var calendar = new Calendar();

// Definimos os CommandProcessors, um para cada método
var commandProcessor1 = new ReflectionCommandProcessor(
    calendar,
    nameof(AddReminderAsync),
    true,
    addReminderOutputProcessor,
    syntax1);
var commandProcessor2 = new ReflectionCommandProcessor(
    calendar,
    nameof(AddReminderForDateAsync),
    true,
    addReminderOutputProcessor,
    syntax2);
var commandProcessor3 = new ReflectionCommandProcessor(
    calendar,
    nameof(AddReminderForDateAndTimeAsync),
    true,
    addReminderOutputProcessor,
    syntax3);

// Criamos o TextProcessor onde os CommandProcessors estarão registrados
var textProcessor = new TextProcessor();
textProcessor.CommandProcessors.Add(commandProcessor1);
textProcessor.CommandProcessors.Add(commandProcessor2);
textProcessor.CommandProcessors.Add(commandProcessor3);

// Por último, incluímos alguns PreProcessors para normalizar a entrada
textProcessor.TextPreprocessors.Add(new TextNormalizerPreprocessor());
textProcessor.TextPreprocessors.Add(new ToLowerCasePreprocessor());

```

E é isso, seu bot já esta pronto para funcionar como um assistente virtual básico:

```csharp
try
{
    var inputText = Console.ReadLine();
    await textProcessor.ProcessAsync(inputText, new RequestContext(), CancellationToken.None);
}
catch (MatchNotFoundException)
{
    Console.WriteLine("Não entendi o que você quis dizer.");
}
```

<img class="alignnone size-full wp-image-166" src="https://andrebires.files.wordpress.com/2016/10/textc1.png" alt="textc.png" width="979" height="512" />

No próximo post, iremos mostrar como otimizar este bot utilizando algoritmos de aproximação e o contexto da conversa.

O código do Textc e deste e outros exemplos estão <a href="https://github.com/takenet/textc-csharp/tree/master/src/Takenet.Textc.Samples">no Github</a>. E você pode conversar com este bot no <a href="https://telegram.me/calendario_textc_bot">Telegram</a>.