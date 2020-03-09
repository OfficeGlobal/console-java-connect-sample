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
# Intégrer l'API Microsoft Graph dans une application de ligne de commande Java à l'aide du nom d'utilisateur et du mot de passe

Cet exemple montre une application de ligne de commande qui appelle l'API Microsoft Graph sécurisée par Azure AD. L’application utilise la [bibliothèque d’authentification ScribeJava](https://github.com/scribejava/scribejava) pour obtenir un jeton Web JSON (JWT) à l’aide du protocole OAuth 2.0. Le JWT renvoyé est appelé **jeton d’accès**. Le jeton d'accès est ajouté à toutes les demandes sur l'API Microsoft Graph en tant qu'en-tête HTTP. Il authentifie l’utilisateur et obtient l’accès au service.

Cet exemple montre comment utiliser **ScribeJava** pour authentifier les utilisateurs à l’aide d’identifiants simples (nom d’utilisateur et mot de passe) et d’une interface textuelle.

## Guide de démarrage rapide

Débuter avec cet exemple est très simple. Il est configuré pour fonctionner immédiatement avec une configuration minimale.

### Étape 1 : Enregistrer un locataire Azure AD

Pour utiliser cet exemple, vous avez besoin d’un locataire Azure Active Directory. Si vous ne savez pas ce qu’est un locataire ou comment en obtenir un, consultez la rubrique [Qu’est-ce qu’un locataire Azure AD](http://technet.microsoft.com/library/jj573650.aspx)? ou [S'inscrire à Azure en tant qu'organisation](http://azure.microsoft.com/documentation/articles/sign-up-organization/). Ces documents vous aideront à prendre en main Azure AD.

### Étape 2 : Téléchargez Java (version 7 et ultérieures) pour votre plateforme

Pour utiliser cet exemple, vous avez besoin d’une installation fonctionnelle [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html) et [Gradle](https://gradle.org/).

L’exemple a été créé à l’aide du plug-in Gradle pour l’IDE IntelliJ IDEA. Le fichier build.gradle est configuré pour vous permettre d’exécuter l’exemple dans IntelliJ. La configuration de création d’exécution vous permet d’exécuter ou de déboguer l’exemple.

### Étape 3 : Télécharger l’exemple d’application et les modules

Clonez ensuite l’exemple du référentiel et installez les dépendances du projet.

À partir de votre shell ou de la ligne de commande :

* `$ git@github.com:microsoftgraph/console-java-connect-sample.git`
* `$ cd console-java-connect-sample`

### Étape 4 : Enregistrer l'application Console Java Connect

1. Connectez-vous au [Portail Azure : inscription des applications](https://go.microsoft.com/fwlink/?linkid=2083908) à l'aide votre compte personnel, professionnel ou scolaire.

2. Sélectionnez **Nouvelle inscription**.

3. Dans la section **Nom**, saisissez un nom d’application explicite qui s’affichera pour les utilisateurs de l’application.

1. Dans la section **Types de comptes pris en charge**, sélectionnez **Comptes dans un annuaire organisationnel et comptes personnels Microsoft (par ex. Skype, Xbox, Outlook.com)**  

1. Sélectionnez **S’inscrire** pour créer l’application. 
	
   La page de présentation de l’application affiche les propriétés de votre application.

4. Copiez l'**ID de l’application (client)**. Il s’agit de l’identificateur unique de votre application. 

1. Dans la liste des pages de l’application, sélectionnez **Authentification**.

1. Sous **URI de redirection** dans la section **URI de redirection suggéré pour les clients publics (mobile, ordinateur de bureau)**, cochez la case située près de **https://login.microsoftonline.com/common/oauth2/nativeclient**

8. Sélectionnez **Enregistrer**.

> **Remarque :** Le point de terminaison d’autorisation Azure Active Directory v2.0 utilise [le consentement incrémentiel et dynamique](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) ce qui signifie que vous n’avez pas besoin de définir des autorisations (désormais appelées **étendues**) lorsque vous enregistrez votre application. Les étendues sont demandées par l’application au moment de l’exécution.

### Étape 5 : Configure l’application à l’aide de Constants.java

À l’aide de votre éditeur de code favori, ouvrez **..\\console-java-connect-sample\\src\\main\\java\\com\\microsoft\\graphsample\\connect\\Constants.java**. Collez, à la ligne 11, l’ID d’application à partir du presse-papiers dans Constants.java, pour remplacer `ENTER_YOUR_CLIENT_ID`.

```java
public class Constants {

    public final static String CLIENT_ID = "ENTER_YOUR_CLIENT_ID";

//...
}
```

### Étape 6 : Installer Gradle

Si le système de génération Gradle n’est pas installé, [installer Gradle](https://docs.gradle.org/4.6/userguide/installation.html).

### Étape 7 : Ajouter le wrapper Gradle au projet

À partir de votre shell ou de la ligne de commande, à la racine du projet :

```Shell
gradle wrapper
```

### Étape 8 : Créer et exécuter l’exemple

À partir de votre shell ou de la ligne de commande, à la racine du projet :

```Shell
gradle run
```

Cette opération exécute l’exemple.

### Exécution de l’exemple

L’interface de ligne de commande ouvre une fenêtre de navigateur sur le point de terminaison d’autorisation Azure Active Directory. Saisissez votre nom d'utilisateur et votre mot de passe pour vous authentifier. Lorsque vous êtes authentifié, vous êtes redirigé vers une fenêtre d’autorisation pour l’exemple d’application. Lisez et acceptez les étendues d’autorisations requises par l’exemple d’application. Cliquez sur le bouton OK dans la fenêtre d’autorisation. Le navigateur accède à `https://login.microsoftonline.com`. Copiez l’adresse complète de l’URL de redirection et collez-la dans un éditeur de texte. Elle devrait ressembler à l'url suivante :

```http
https://login.microsoftonline.com/common/oauth2/nativeclient?code={IAQABAAIAAABHh4kmS_aKT5XrjzxRAtHz5S...p7OoAFPmGPqIq-1_bMCAA}&session_state=dd64ce71-4424-494b-8818-be9a99ca0798
```

> **Remarque :** L'exemple d'URL est tronqué pour plus de clarté. `{` et `}` ont été ajoutées à l’exemple pour délimiter le code d’autorisation à des fins d’illustration.

Le code d’autorisation commence après `?code=` et se termine avant `&session_state`.

Copiez le code d’autorisation dans le presse-papiers du système et collez-le dans la console sous `Et collez le code d’autorisation ici`.

Le message suivant s’affiche : 

```Shell
Hello, {your name}. Would you like to send an email to yourself or someone else?
Enter the address to which you'd like to send a message. If you enter nothing, the message will go to your address
```

Lorsque vous avez saisi une adresse e-mail ou laissé le champ vide, un e-mail vous est envoyé à vous ou à l’adresse que vous avez saisi dans la console.

Vous pouvez envoyer un e-mail à d’autres adresses en répondant par « o » à l’invite `Voulez-vous envoyer un autre message ? Tapez ’o’ pour Oui et une autre touche pour quitter.`
