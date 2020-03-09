---
page_type: sample
products:
- office-outlook
- office-word
- ms-graph
languages:
- java
extensions:
  contentType: samples
  technologies:
  - Microsoft Graph 
  - Microsoft identity platform
  services:
  - Outlook
  - Microsoft identity platform
  platforms:
  - Android
  createdDate: 3/5/2018 10:29:43 AM
---
# Integrar a API do Microsoft Graph a um aplicativo de linha de comando Java usando nome de usuário e senha

Este exemplo mostra um aplicativo de linha de comando que chama a API do Microsoft Graph protegida no Azure AD. O aplicativo usa a [biblioteca de autenticação ScribeJava](https://github.com/scribejava/scribejava) para obter um token Web JSON (JWT) por meio do protocolo OAuth 2.0. O JWT retornado é conhecido como um **token de acesso**. O token de acesso é adicionado a todas as solicitações na API do Microsoft Graph como um cabeçalho HTTP. Ele autentica o usuário e obtém acesso ao serviço.

Este exemplo mostra como usar o **ScribeJava** para autenticar os usuários por meio de credenciais simples (nome de usuário e senha) usando uma interface somente texto.

## Início Rápido

É fácil começar com o exemplo. Está configurada para ser executado na caixa com a configuração mínima.

### Etapa 1: Registrar um locatário do Azure AD

Para usar esse exemplo, você precisará de um locatário do Azure Active Directory. Caso não saiba ao certo o que é um locatário ou como obter um, leia [O que é um locatário do Azure AD](http://technet.microsoft.com/library/jj573650.aspx)? ou [Inscreva-se no Azure como uma organização](http://azure.microsoft.com/documentation/articles/sign-up-organization/). Esses documentos devem ajudá-lo a começar a usar o Azure AD.

### Etapa 2: Baixar Java (7 e acima) para sua plataforma

Para usar este exemplo com êxito, você deve ter uma instalação do [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html) e do [Gradle](https://gradle.org/) funcionando corretamente.

O exemplo foi criado usando o plug-in Gradle para IntelliJ IDEA IDE. O arquivo build.gradle do projeto é configurado para permitir que você execute o exemplo de dentro do IntelliJ. A configuração executar build permite que você execute ou depure o exemplo.

### Etapa 3: Baixar o aplicativo de exemplo e os módulos

Em seguida, clone o repositório de exemplo e instale as dependências do projeto.

A partir do seu shell ou linha de comando:

* `$ git@github.com:microsoftgraph/console-java-connect-sample.git`
* `$ cd console-java-connect-sample`

### Etapa 4: Registrar o aplicativo Console Java Connect

1. Entre no [Portal do Azure – Registros do Aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) usando sua conta pessoal, sua conta corporativa ou de estudante.

2. Escolha **Novo registro**.

3. Na seção **Nome**, insira um nome de aplicativo relevante que será exibido aos usuários do aplicativo

1. Na seção **Tipos de conta com suporte**, selecione **Contas em qualquer diretório organizacional e contas pessoais da Microsoft (por exemplo, Skype, Xbox, Outlook.com)**.  

1. Selecione **Registrar** para criar o aplicativo. 
	
   A página Visão Geral do aplicativo mostra as propriedades do seu aplicativo.

4. Copie a **ID do aplicativo (cliente)**. Esse é o identificador exclusivo do aplicativo. 

1. Na lista de páginas do aplicativo, selecione **Autenticação**.

1. Em **Redirecionamento de URIs** na seção **URIs redirecionadas sugeridas para clientes públicos (móvel, área de trabalho)**, marque a caixa ao lado de **https://login.microsoftonline.com/common/oauth2/nativeclient**

8. Escolha **Salvar**.

> **Observação:** O ponto de extremidade de autorização do Azure Active Directory v 2.0 usa consentimento incremental e dinâmico o que significa que você não precisa definir permissões (agora chamado de **escopos**) ao registrar seu aplicativo. Os escopos são solicitados pelo seu aplicativo em tempo de execução.

### Etapa 5: Configure seu aplicativo usando Constants.java

Usando seu editor de código favorito, abra **..\\console-java-connect-sample\\src\\main\\java\\com\\microsoft\\graphsample\\connect\\Constants.java**. Cole a ID do aplicativo da área de transferência em Constants.java, linha 11, para substituir `ENTER_YOUR_CLIENT_ID`.

```java
public class Constants {

    public final static String CLIENT_ID = "ENTER_YOUR_CLIENT_ID";

//...
}
```

### Etapa 6: Instalar o Gradle

Se você não tiver o sistema de compilação Gradle instalado, [instale o Gradle](https://docs.gradle.org/4.6/userguide/installation.html).

### Etapa 7: Adicionar o wrapper do Gradle ao projeto

A partir do seu shell ou linha de comando na raiz do projeto:

```Shell
gradle wrapper
```

### Etapa 8: Criar e executar o exemplo

A partir do seu shell ou linha de comando na raiz do projeto:

```Shell
gradle run
```

Isso executará o exemplo.

### Execução do exemplo

A interface da linha de comando abre uma janela de navegador no ponto de extremidade de autorização do Azure Active Directory. Insira o nome de usuário e a senha para autenticação. Quando estiver autenticado, você será levado a uma janela de autorização para o aplicativo de exemplo. Analise e aceite os escopos de permissão solicitados pelo aplicativo. Clique no botão OK na janela de autorização. O navegador navega para `https://login.microsoftonline.com`. Copie o endereço da URL de redirecionamento completo e cole-o em um editor de texto. Ele deverá ser semelhante a url a seguir:

```http
https://login.microsoftonline.com/common/oauth2/nativeclient?code={IAQABAAIAAABHh4kmS_aKT5XrjzxRAtHz5S...p7OoAFPmGPqIq-1_bMCAA}&session_state=dd64ce71-4424-494b-8818-be9a99ca0798
```

> **Observação:** O exemplo de URL é truncado para fins de clareza. `{` e `}` foram adicionados ao exemplo para delimitar o código de autorização para fins ilustrativos.

O código de autorização inicia após `?code=` e termina antes de `& session_state`.

Copie o código de autorização para a área de transferência do sistema e cole-o no console em `E cole o código de autorização aqui`.

É solicitado se você deseja confirmar:

```Shell
Hello, {your name}. Would you like to send an email to yourself or someone else?
Enter the address to which you'd like to send a message. If you enter nothing, the message will go to your address
```

Depois de inserir um endereço de e-mail ou deixar o aviso em branco, um e-mail é enviado para você ou o endereço digitado no console.

Você pode enviar e-mails para outros endereços respondendo com "s" à solicitação `Deseja enviar outra mensagem? Digite 's' para Sim e qualquer outra tecla para sair.`
