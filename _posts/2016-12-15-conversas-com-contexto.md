---
title:  "Conversas com contexto"
date:   2016-12-15 13:26:00 +0000
categories: [Casos de uso]
tags: [Textc, C#]
author: andreb
---

Numa conversa natural, o **contexto** é algo extremamente importante e quando não devidamente estabelecido, pode levar a mal entendidos.

<!--preview--> 

Uma mesma sentença pode ter um significado diferente dependendo, por exemplo, do *lugar e horário* onde foi dita ou da *idade e sexo* da pessoa que se está conversando. Além disso, faz parte do contexto informações compartilhadas em outros momentos, como preferências das pessoas envolvidas e o tema atual da conversa.

Por exemplo, imagine o seguinte diálogo:

```
João: Qual a marca do seu carro?
Maria: Meu carro é uma BMW.
João: E qual a cor?
Maria: É amarelo. 
```

Na segunda pergunta, *João* não precisou especificar que ele estava se referindo ao carro de *Maria*, pois esta informação estava implicita no contexto, sendo que sua pergunta na verdade foi *Qual a cor do seu carro?*.

Ao construir um chatbot, deve se considerar que o usuário poderá tentar se comunicar da maneira que ele conversa com outras pessoas da sua lista de contatos