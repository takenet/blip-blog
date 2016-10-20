---
title:  "Construíndo um bot assistente virtual utilizando o Textc"
date:   2016-10-17 15:04:47 +0000
categories: textc c#
---

Os bots de mensagem - ou **chatbots** - são programas que permitem a interação através de mensagens para oferecer algum tipo de serviço. Apesar de não ser uma tecnologia nova, recentemente vem ganhando destaque nas notícias desde que o Facebook, Microsoft e outras empresas anunciaram investimentos em plataformas para construção deste tipo de aplicação.

<!--preview--> 

Um dos principais argumentos para o uso dos bots é que o modelo de **distribuição de serviços através de aplicativos móveis está saturado**. Existe uma resistência por parte dos usuários de instalar novos aplicativos para uso de serviços simples. Por exemplo, imagine que se cada pizzaria de sua cidade oferecesse um aplicativo para realizar o pedido de pizza, você precisaria ter aplicativos diferentes para pedir pizza em lugares variados. Mas e se você não precisasse instalar nenhum aplicativo novo e **utilizar os próprios aplicativos de mensagem que você (provavelmente) já possui para este fim**? É esta oportunidade que o Facebook, Microsoft, Google, Slack e outras grandes empresas estão tentando explorar.

Oferecer uma interface amigável aos usuários de bots é um grande desafio, já que a forma de entrada padrão disponível é o texto que por sua vez é pouco estruturado. Afinal, **não há garantia do que o usuário vai escrever**, nem a forma, gramática, sintaxe, etc. Existem algumas soluções que tentam contornar esta limitação - como o uso de botões nos canais de mensagem (como Telegram e Messenger) que conduzem a navegação do usuário ou o uso de de inteligência artificial para tentar "adivinhar" o que o usuário está querendo dizer. Cada solução tem seus prós e contras e neste artigo iremos demonstrar como fazer isso utilizando a **biblioteca de processamento de linguagem natural [Textc](https://github.com/takenet/textc-csharp)**.

De maneira resumida, a **Textc permite a definição de sintaxes de texto e associá-las a chamadas de métodos de uma classe**. Uma sintaxe define uma estrutura de texto, com tokens e seus tipos, sendo cada token mapeado a parâmetros de um método. Para ilustrar iremos construir um **assistente virtual que permite o armazenamento de lembretes**, de forma semelhante ao que o Google Now oferece.

## Mãos a obra

Antes de tudo, imagine que temos a classe `Calendar` abaixo:

```csharp
public class Calendar
{
    public async Task<string> AddReminderAsync(string reminder, string date, string time)
    {
        // TODO: Save reminder for the specified date/time and return a message to the user
    }
}
```

O método `AddReminderAsync` desta classe deverá chamada pelo Textc para o armazenamento dos lembretes, retornando um texto de resposta ao usuário. Devemos agora configurar o Textc para realizar esta chamada a partir das entradas dos usuários.

O primeiro passo é enumerar as diferentes formas que o usuário poderá solicitar o armazenamento de um lembrete. Alguns exemplos:

1. *Lembrar de ir ao médico*
2. *Lembre me amanhã de pagar a conta de luz*
3. *Me lembre de fazer compras hoje a tarde*

São estruturas de texto diferentes mas que possuem as mesmas informações, que são:
- O comando para adicionar um lembrete: *Lembrar de, Lembre me, Me lembre de*
- O texto do lembrete: *ir ao médico, pagar a conta de luz, fazer compras*
- A data do lembrete: *amanhã, hoje a tarde*

Cada informação é mapeada em um ou mais **tokens** e conjunto destes é chamado de **sintaxe**. Isolando as informações presentes nos exemplos acima, temos:
1. (lembrar de) (ir ao médico) 
2. (Lembre me) (amanhã de) (pagar a conta de luz) 
3. (Me lembre de) (fazer compras) (hoje a tarde)

Para representarmos as sintaxes e seus tokens no Textc, utilizaremos a CSDL - *Command Syntax Definition Language* - uma notação simples oferecida pela biblioteca. Uma declaração CSDL é constituída de um ou mais tokens, sendo cada token representado da seguinte forma:

```
name:Type(initializer)
```

Onde:
- **name** - O nome do token que será extraido da entrada. Este valor pode ser utilizado no mapeamento com os parâmetros do método de uma classe. Opcional.
- **type** - O tipo do token no texto. A biblioteca define alguns tipos como `Word` (uma palavra), `Text` (uma ou mais palavras) e `Integer` (número inteiro). Obrigatório.
- **initializer** - Valor de inicialização, sendo utilizado para limitar os valores válidos para o tipo. Por exemplo, no tipo `Word`, determina quais são as palavras válidas para serem consideradas na entrada do usuário. Opcional.

A representação da primeira sintaxe fica da seguinte forma:

```
:Word(lembrar) :Word?(de) reminder:Text
```

Os dois primeiros tokens são anônimos pois a informação dos mesmos não será utilizada na chamada do método de nossa classe e por isso podem ser ignoradas. Alguns tokens podem ser marcados como **opcionais** em uma sintaxe, bastando incluir um ponto-de-interrogação depois da declaração do tipo - como fizemos na preposição *de* acima. Neste caso, a sintaxe é válida para entradas como *lembrar de ir ao médico* ou *lembrar médico*.

A segunda sintaxe é semelhante a primeira, com uma informação adicional - a data do lembrete. Ela também inclui outros tokens que não estão presentes na primeira mas que apenas constituem a estrutura do texto. Representando-a com CSDL, temos:

```
:Word(lembre) :Word?(me) date:Word?(hoje,amanha,eventualmente) :Word?(de) reminder:Text
```

Por fim, precisamos configurar a terceira sintaxe, que a princípio parece simples:

```
:Word?(me) :Word(lembre) :Word?(de) reminder:Text date:Word?(hoje,amanha,eventualmente) :Word?(a) time:Word?(manha,tarde,noite)
```

Só que existe um problema: desta forma, o token **reminder** capturará todo o restante da entrada do usuário, e nunca teríamos *match* dos demais tokens à direita do mesmo (*date* e *time*). Isso acontece porque o tipo de token **Text** é **guloso** ou seja, consome todo o restante da entrada do usuário. Por este motivo, ele deve ser o último token a ser processado em uma sintaxe. Por padrão, o processamento ocorre da **esquerda para a direita**, mas podemos alterar a direção em qualquer ponto da sintaxe ao incluirmos o modificador **~** (til) após o tipo de um token. 

Como precisamos que o token **reminder** seja o último a ser processado, a direção de parse deve mudar após o processamento do token imediatamente a esquerda deste - no caso a palavra *de*. Neste caso, teríamos:

```
:Word?(me) :Word(lembre) :Word~(de) reminder:Text date:Word?(hoje,amanha,eventualmente) :Word?(a) time:Word?(manha,tarde,noite)
```

Assim, logo após o *match* da palavra *de*, o parse continuará a partir do final da sintaxe, buscando no final da entrada o valor do token *time*.