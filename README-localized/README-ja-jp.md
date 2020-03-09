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
# ユーザー名とパスワードを使用して、Microsoft Graph API を Java コマンド ライン アプリに統合します。

このサンプルでは、Azure AD でセキュリティ保護された Microsoft Graph API を呼び出すコマンド ライン アプリを示します。アプリは、[ScribeJava 認証ライブラリ](https://github.com/scribejava/scribejava)を使用して、OAuth 2.0 プロトコルを使用して、JSON Web Token (JWT) を取得します。返される JWT は、**アクセス トークン**と呼ばれます。アクセス トークンは、Microsoft Graph API のすべての要求に HTTP ヘッダーとして追加されます。ユーザーを認証し、サービスへのアクセス権を取得します。

このサンプルでは、**ScribeJava** を使用して、シンプルな資格情報 (ユーザー名とパスワード) を使用して、テキストのみのインターフェイスを使用してユーザーを認証する方法について説明します。

## クイック スタート

サンプルの使用を開始するための手順は簡単です。最小のセットアップですぐに動作するように構成されています。

### 手順 1: Azure AD テナントを登録する

このサンプルを使用するには、Azure Active Directory テナントが必要です。テナントの内容または入手方法がわからない場合は、「[Azure AD テナントとは?](http://technet.microsoft.com/library/jj573650.aspx)」または「[組織として Azure にサインアップする](http://azure.microsoft.com/documentation/articles/sign-up-organization/)」をお読みください。これらのドキュメントにより、Azure AD の使用を開始することができます。

### 手順 2:プラットフォーム用の Java (7 以降) をダウンロードする

このサンプルを使用するには、実稼働する [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html) および [Cradle](https://gradle.org/) のインストール環境が必要になります。

サンプルは IntelliJ IDEA IDE. の Gradle プラグインを使用して構築されています。プロジェクトの build.gradle ファイルは、IntelliJ 内からサンプルを実行できるように構成されています。ビルド構成の実行により、サンプルを実行またはデバッグできます。

### 手順 3:サンプル アプリケーションとモジュールをダウンロードする

次に、サンプル リポジトリのクローンを作成し、プロジェクトの依存関係をインストールします。

シェルまたはコマンド ラインから:

* `$ git@github.com:microsoftgraph/console-java-connect-sample.git`
* `$ cd console-java-connect-sample`

### 手順 4: Console Java Connect アプリを登録する

1. 個人用アカウントか職場または学校アカウントのいずれかを使用して、[Azure Portal の [アプリ登録]](https://go.microsoft.com/fwlink/?linkid=2083908) にサインインします。

2. [**新規登録**] を選択します。

3. [**名前**] セクションに、アプリのユーザーに表示されるわかりやすいアプリケーション名を入力します。

1. [**サポートされているアカウントの種類**] セクションで、[**組織ディレクトリ内のアカウントと個人の Microsoft アカウント (例: Skype、Xbox、Outlook.com)**] を選択します。  

1. [**登録**] を選択して、アプリケーションを作成します。 
	
   アプリケーションの概要ページには、アプリのプロパティが表示されます。

4. **アプリケーション (クライアント) ID** をコピーします。これは、アプリの一意識別子です。 

1. アプリケーションのページの一覧から [**認証**] を選択します。

1. [**パブリック クライアント (モバイル、デスクトップ) 用の推奨リダイレクト URI**] セクションの [**リダイレクト URI**] の下で、 **https://login.microsoftonline.com/common/oauth2/nativeclient** の隣りのボックスにチェックを入れます。

8. [**保存**] を選択します。

> **注:**Azure Active Directory v2.0 認証エンドポイントは、[増分および動的な同意](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent)を使用します。つまり、アプリケーシを登録するとき、アクセス許可 (現在では**適用範囲**と呼ばれます) を設定する必要はありません。適用範囲は実行時にアプリによって要求されます。

### 手順 5: Constants.java を使用してアプリを構成する

お気に入りのコード エディターを使用して、**..\\console-java-connect-sample\\src\\main\\java\\com\\microsoft\\graphsample\\connect\\Constants.java** を開きます。Constants.java にクリップボードからアプリケーシ ID を貼り付け、行 11 を `ENTER_YOUR_CLIENT_ID` に置き換えます。

```java
public class Constants {

    public final static String CLIENT_ID = "ENTER_YOUR_CLIENT_ID";

//...
}
```

### 手順 6: Gradle をインストールする

Gradle ビルド システムをインストールしていない場合は、[Gradle をインストールしてください](https://docs.gradle.org/4.6/userguide/installation.html)。

### 手順 7:プロジェクトに Gradle ラッパーを追加する

プロジェクト ルートで、シェルまたはコマンド ラインから:

```Shell
gradle wrapper
```

### 手順 8:サンプルのビルドと実行

プロジェクト ルートで、シェルまたはコマンド ラインから:

```Shell
gradle run
```

これによりサンプルが実行されます。

### サンプルの実行

コマンド ライン インターフェイスは、Azure Active Directory 承認エンドポイントでブラウザー ウィンドウを開きます。ユーザー名とパスワードを入力し認証します。認証が完了すると、サンプル アプリの承認ウィンドウが表示されます。アプリによって要求された適用範囲を確認して、承諾します。認証ウィンドウの [OK] ボタンをクリックします。ブラウザーが `https://login.microsoftonline.com` に移動します。完全なリダイレクト URL アドレスをコピーし、テキスト エディターに貼り付けます。次のように URL が表示されます。

```http
https://login.microsoftonline.com/common/oauth2/nativeclient?code={IAQABAAIAAABHh4kmS_aKT5XrjzxRAtHz5S...p7OoAFPmGPqIq-1_bMCAA}&session_state=dd64ce71-4424-494b-8818-be9a99ca0798
```

> **注:**わかりやすくするために、URL の例は省略されています。 `{` および `}` は、説明のために認証コードを区切るために例に追加されました。

認証コードは、`?code=` の後から始まり、`&session_state` の前に終了します。

システム クリップボードに認証コードをコピーし、「`ここに認証コードを貼り付ける`」の下のコンソールに貼り付けます。

次のメッセージが表示されます。

```Shell
Hello, {your name}. Would you like to send an email to yourself or someone else?
Enter the address to which you'd like to send a message. If you enter nothing, the message will go to your address
```

メールアドレスを入力するか、空白のままにすると、メールが自分自身またはコンソールに入力したアドレスに送信されます。

次のメッセージが表示されたとき、"y" で回答して、他のアドレスにメールを送信できます「`別のメッセージを送信しますか?はいの場合は 'y' を入力し、終了する場合は他のキーを入力してください。`」
