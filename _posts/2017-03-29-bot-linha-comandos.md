Seu bot não deve ser um prompt de linha de comandos
===================================================

No início do século passado, sugiram as máquinas de [Teletipo](https://pt.wikipedia.org/wiki/Teletipo) que eram utilizadas para a troca de mensagens de texto entre pessoas em localizações remotas. Estas máquinas permitiam que um originador digitasse a mensagens em seu terminal - com um teclado QWERTY - para que um destinatário recebesse a mesma de forma impressa. O Teletipo veio como uma alternativa ao código morse e o telégrafo que até então eram a forma mais popular para o envio de mensagens em tempo real. 

<img src="https://upload.wikimedia.org/wikipedia/commons/8/89/WACsOperateTeletype.jpg"  />

Naquela época, o Teletipo era utilizado exclusivamente para a comunicação entre pessoas, mas em meados do século XX - com o surgimento dos computadores - as primeiras máquinas passaram a "conversar" através desta interface, enviando respostas a comandos dos operadores humanos, que eram impressas no terminal do operador. Este foi o surgimento da [interface de linha de comandos](https://pt.wikipedia.org/wiki/Interpretador_de_comandos) ou **CLI** (*command line interface*), mesmo que ainda de forma primitiva.

Posteriomente, o Teletipo foi sendo substituido pelo [Terminal burro](https://pt.wikipedia.org/wiki/Terminal_(inform%C3%A1tica)) que permitia o recebimento das mensagens em uma tela, ao invés da impressora. Por fim, os terminais burros ficaram inteligentes, recebendo um microprocessador, que os permitia responder aos comandos sem a necessidade de um computador remoto para processar. Surgia então o **computador pessoal**.

<img src="https://upload.wikimedia.org/wikipedia/commons/2/29/Linux_command-line._Bash._GNOME_Terminal._screenshot.png" width="500px" />

Nos anos 80, os primeiros computadores pessoais começaram a se popularizar, inicialmente utilizando uma interface de linha de comandos - como no IBM PC e no Apple II. Por este motivo, os primeiros PCs eram considerados [**intimidadores para novos usuários**](http://www.computerhope.com/issues/ch000619.htm) já que interface de texto possui uma curva de aprendizado alta e requer que o usuário memorize comandos para serem submetidos através do teclado. Para tentar resolver este problema, foi desenvolvida pouco depois a [interface gráfica de usuário](https://pt.wikipedia.org/wiki/Interface_gr%C3%A1fica_do_utilizador) ou **GUI** (*graphic user interface*), que traz elementos visuais que os usuários reconhecem - como ícones e botões - afim de diminuir a curva de aprendizado.

<img src="https://upload.wikimedia.org/wikipedia/en/1/1d/Xerox_Star_8010_workstations.jpg" />

Pode-se dizer que a popularização em massa dos computadores só foi possível depois do surgimento da interface gráfica do usuário. Hoje temos inúmeros tipos de dispositivos que utilizam esse paradigma - celulares, tablets, geladeiras, etc. Apesar disso, a interface de linha de comando ainda é utilizada, mas em nichos específicos, como desenvolvedores e administradores de sistema, devido a sua flexibilidade e poder. Para estes casos, existem situações que é muito mais produtivo e rápido utilizar um comando que uma ferramenta visual. Mas é indiscutível que para o usuário comum, uma interface gráfica é algo muito mais amigável que requer um esforço muito menor para a realização de tarefas comuns.

Hoje, com a popularização dos **bots de mensagem** (chatbots), vivemos algo parecido com o velho embate **CLI vs GUI**: será que o melhor é construir um bot que receba, de maneira aberta e flexível, comandos de texto do usuário para que tente interpretá-los em ações ou o melhor seria utilizar elementos visuais, como botões, imagens e menus para apresentar ao usuário as ações possível naquele momento, mesmo que limitando suas opções?

Se aprendemos com a história, poderiamos dizer que a resposta é a mesma: na maioria dos casos, uma **interface com elementos gráficos é muito mais amigável para o usuário comum**. Mas podem haver casos bastante específicos que um bot com interface de texto pode ser a melhor opção. 

Um ponto que na minha opinião é bastante mal entendido é a natureza
*conversacional* dos bots, que é uma das grandes sacadas deste tipo de aplicação. Muitas pessoas entendem isso como o fato de você poder conversar com um robô como se fosse uma pessoa. Mas na verdade, está muito mais relacionado a possibilidade do usuário **voltar na linha do tempo das interações através da thread** com o bot, **mesmo que tenha sido através de botões e elementos visuais**. Este recurso posibilita a criação de um contexto mais rico da interação entre a pessoa e a aplicação.

Portanto, se seu usuário não for um hacker, não construa seu bot como um prompt de linha de comandos.
