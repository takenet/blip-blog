---
title:  "Blip Chat Mobile"
date:   2017-05-22 15:30:00 +0000
categories: [Novidades]
tags: [releases, mobile]
author: victorb
---

Com o **BLiP Chat** agora você pode colocar o seu chatbot dentro do seu aplicativo Android e iOS de forma super simples.

<!--preview-->

<figure>
    <img class="alignnone size-full" src="/assets/posts/2017-05-15-blip-chat-mobile/blip-chat-initial.jpg" height="490" alt="BLiP Chat Android e iOS" />
</figure>

# Android
Para utilizar o BLiP Chat dentro do seu app Android, você pode escolher o método de instalação. Para isso, adicione a referência do repositório no arquivo *build.gradle* do seu projeto:
```groovy
allprojects {
    repositories {
        //others repository dependencies
        maven { url 'http://dl.bintray.com/takenet/maven' }
    }
}
```
E importe o módulo via gradle:
```groovy
compile 'net.take:blip-chat:0.0.25'
```
ou via Maven:
```xml
<dependency>
  <groupId>net.take</groupId>
  <artifactId>blip-chat</artifactId>
  <version>0.0.22</version>
  <type>pom</type>
</dependency>
```

# iOS
No iOS, o BLiP Chat suporta aplicativos feitos em *Swift* e *Objective-C*. A instalação é feita através do CocoaPod. Caso ainda não tenha o CocoaPod confira [este guia](https://guides.cocoapods.org/using/using-cocoapods.html) ensinando a configurá-lo. Para usar o BLiP Chat, basta adicionar a referência no arquivo *Podfile*:
```ruby
target 'MyApp' do
    ...
    pod "BlipChat"
    ...
end
```
E concluir a instalação do pod rodando o comando no diretório de seu projeto:
```ruby
$ pod install
```
# Pré-requisitos
Para poder utilizar o BLiP Chat, seu app deve ter acesso a **Internet**, sendo que no Android esse acesso precisa ser requisitado dentro do *AndroidManifest.xml*. Para isso, adicione a permissão de internet no seu aplicativo. Se o seu chatbot em algum momento requisita a **localização** do usuário, você também deve adicionar a permissão de localização.
```xml
<manifest xlmns:android...>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    ...
</manifest>
```

No iOS apenas a permissão de localização precisa ser informada. Então, caso seu chatbot  em algum momento requisite a **localização** do usuário, você deve adicionar uma mensagem para o usuário explicando porque a localização é necessária. Adicione a chave *Privacy - Location When In Use Usage Description* no arquivo **info.plist** do seu projeto.

<figure>
    <img class="alignnone size-full" src="/assets/posts/2017-05-15-blip-chat-mobile/location-request.png" alt="info.plist" />
</figure>

# Configurando seu Chat
Para incluir o seu chatbot em seu aplicativo, você precisa pegar a sua ApiKey. Caso tenha dúvidas, você pode conferir este [post ensinando a fazer isso](http://blog.blip.ai/2017/04/05/novidades-plataforma.html).

## Abrindo sua janela de conversa
Para abrir uma conversa com o seu chatbot é muito simples. Use a classe **BlipClient** e chame o método *openBlipThread* passando o seu contexto atual e sua ApiKey. 

Android:
```java
BlipClient.openBlipThread(context, "SUA-API-KEY", null);
```

iOS:
```swift
BlipClient.openBlipThread(myView: self, apiKey: "SUA-API-KEY", options: nil)
```
O método *openBlipThread* pode retornar uma exceção, já que ele verifica se possui todas as informações necessárias para abrir o chat, então você deve colocá-lo dentro de um *try catch*.
Você também pode passar um objeto *BlipOptions*, que possui algumas configurações opcionais que serão abordadas mais abaixo.

Imagine que você queira, por exemplo, abrir uma conversa entre o usuário e seu chatbot assim que o app é aberto.

Android:
```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        try {
            BlipClient.openBlipThread(context, "SUA-API-KEY", null);
        } catch (IllegalArgumentException e) {
            e.printStackTrace();
        }
    }
}
```
Swift:
```swift
class ViewController: UIViewController {

    override func viewDidAppear(_ animated: Bool) {
        do {
            try BlipClient.openBlipThread(myView: self, apiKey: "SUA-API-KEY", options: nil)
        } catch {
            print (error.localizedDescription)
        }
    }
}
```

# Customizando o seu Chat
Existem algumas possibilidades de customização do seu Chat que podem ser configuradas através do objeto **BlipOptions**. 

## Autenticação
É possível definir o tipo de autenticação do usuário que irá conversar com o seu chatbot. Existem três tipos de autenticações possíveis. 
* Guest, onde cada usuário é tratada como convidado e não há quaisquer informações sobre o usuário.
* Login, onde o usuário deve informar seu *nome* e *email* antes de conversar com o chatbot.
* Dev, onde o desenvolvedor do app é responsável por passar as informações do usuário para o BLiP Chat. Nesse modo o histórico da conversa esta disponível sempre que o usuário se conectar.

Para entender melhor os possíveis modos de autenticação dê uma olhada [nesse post](http://blog.blip.ai/2017/05/16/tipos-autenticacao-blip-chat.html) que explica de forma detalhada cada tipo.

## Esconder o menu da janela
A janela de conversa com o seu chatbot possui um menu no canto superior direito que pode ser escondida. Para isso, basta definir o valor para a propriedade **hideMenu** dentro do objeto *BlipOptions*. Por padrão essa propriedade é *false*.

Android:
```java
BlipOptions blipOptions = new BlipOptions();
blipOptions.setHideMenu(true);
```

iOS:
```swift
let options = BlipOptions()
options.hideMenu = false;
```

## Título da janela
No iOS a janela do BLiP Chat possui um título que pode ser customizado. Para isso, defina o valor da propriedade **windowTitle** com o título apropriado. Por padrão este título é *BLiP Chat*.

```swift
let options = BlipOptions()
options.windowTitle = "Seu Título";
```

# Exemplos
Após conhecer um pouco mais sobre as funcionalidades do BLiP Chat vamos fazer um exemplo um pouco mais completo. Digamos que você queria abrir uma conversa entre o usuário e seu chatbot com o tipo de autenticação **Dev**, fornecendo as informações de nome, email e senha, e esconder o menu da janela de chat.

Android:

```java
import net.take.blipchat.*;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        try {
            BlipOptions blipOptions = new BlipOptions();
            blipOptions.setAuthType(AuthType.DEV);
            blipOptions.setUserIdentifier("IDENTIFICADOR-DO-USUARIO");
            blipOptions.setUserPassword("SENHA-DO-USUARIO");
            blipOptions.setUserName("NOME-DO-USUARIO");
            blipOptions.setUserEmail("EMAIL-DO-USUARIO");
            blipOptions.setHideMenu(true); // Esconde o menu da janela

            BlipClient.openBlipThread(MainActivity.this, "SUA-API-KEY", blipOptions);
        } catch (IllegalArgumentException e) {
            e.printStackTrace();
        }

    }
}
```

iOS:
```swift
import BlipChat

class ViewController: UIViewController {
   
   @IBAction func openThread(_ sender: Any) {
       let options = BlipOptions(authType: .Dev,
                                 userIdentifier: "IDENTIFICADOR-DO-USUARIO",
                                 userPassword: "SENHA-DO-USUARIO",
                                 userName: "NOME-DO-USUARIO",
                                 userEmail: "EMAIL-DO-USUARIO")
       options.windowTitle = "Meu App iOS" // Define o titulo da janela
       options.hideMenu = true // Esconde o menu da janela
       do {
           try BlipClient.openBlipThread(myView: self, apiKey: "SUA-API-KEY", options: options)
       } catch {
           print (error.localizedDescription)
       }
   }
   
}
```
<figure>
    <img class="alignnone size-full" src="/assets/posts/2017-05-15-blip-chat-mobile/blip-chat-dev.jpg" height="490" alt="BLiP Chat Android e iOS" />
</figure>

Ficou com alguma dúvida? Deixe seu comentário ou peça ajuda em nosso [fórum](http://forum.blip.ai/).
