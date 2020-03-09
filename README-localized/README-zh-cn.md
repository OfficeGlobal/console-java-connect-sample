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
# 使用用户名和密码将 Microsoft Graph API 集成到 Java 命令行应用中

此示例显示用于调用受 Azure AD 保护的 Microsoft Graph API 的命令行应用。该应用使用 [ScribeJava 身份验证库](https://github.com/scribejava/scribejava)通过 OAuth 2.0 协议获取 JSON Web 令牌 (JWT)。返回的 JWT 称为**访问令牌**。访问令牌作为 HTTP 标头添加到 Microsoft Graph API 上的所有请求中。它对用户进行身份验证并访问服务。

此示例演示如何使用 **ScribeJava** 和纯文本界面，通过简单的凭据（用户名和密码）对用户进行身份验证。

## 快速入门

示例入门很简单。它配置为只需最少的设置即可开箱运行。

### 步骤 1：注册 Azure AD 租户

若要使用本示例，需要一个 Azure Active Directory 租户。如果不确定什么是租户或者如何获取租户，请阅读[什么是 Azure AD 租户？](http://technet.microsoft.com/library/jj573650.aspx)或[以组织身份注册 Azure](http://azure.microsoft.com/documentation/articles/sign-up-organization/)。这些文档将帮助你开始使用 Azure AD。

### 步骤 2：为你的平台下载 Java（7 及以上版本）

若要使用本示例，需要正确安装 [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 和 [Gradle](https://gradle.org/)。

该示例是使用适用于 IntelliJ IDEA IDE 的 Gradle 插件构建的。项目 build.gradle 文件配置为允许你从 IntelliJ 中运行示例。运行生成配置允许你运行或调试示例。

### 第 3 步：下载示例应用程序和模块

接下来，克隆示例存储库并安装项目的依赖项。

在 shell 或命令行中键入：

* `$ git@github.com:microsoftgraph/console-java-connect-sample.git`
* `$ cd console-java-connect-sample`

### 步骤 4：注册控制台 Java 连接应用

1. 使用你的个人或工作或学校帐户登录 [Azure 门户 - 应用注册](https://go.microsoft.com/fwlink/?linkid=2083908)。

2. 选择“**新注册**”。

3. 在“**名称**”部分输入一个会显示给应用用户的有意义的应用程序名称

1. 在“**支持的帐户类型**”部分，选择“**任何组织目录中的帐户和个人 Microsoft 帐户(例如 Skype、Xbox、Outlook.com)**”  

1. 选择“**注册**”以创建应用程序。 
	
   应用程序的“概述”页面显示了应用程序的属性。

4. 复制**应用程序（客户端）ID**。这是应用的唯一标识符。 

1. 在应用程序的页面列表中，选择“**身份验证**”。

1. 在“**建议用于公共客户端(移动、桌面)的重定向 URI**”部分的“**重定向 URI**”下，选中 **https://login.microsoftonline.com/common/oauth2/nativeclient** 旁边的框

8. 选择“**保存**”。

> **注意：**Azure Active Directory v2.0 授权终结点使用[增量同意和动态同意](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent)，这意味着注册应用程序时无需设置权限（现在称为**作用域**）。你的应用在运行时请求作用域。

### 步骤 5：使用 Constants.java 配置你的应用

使用常用的代码编辑器，打开 **..\\console-java-connect-sample\\src\\main\\java\\com\\microsoft\\graphsample\\connect\\Constants.java**。将应用程序 ID 从剪贴板粘贴到 Constants.java 第 11 行，以替换 `ENTER_YOUR_CLIENT_ID`。

```java
public class Constants {

    public final static String CLIENT_ID = "ENTER_YOUR_CLIENT_ID";

//...
}
```

### 步骤 6：安装 Gradle

如果你尚未安装 Gradle 生成系统，请[安装 Gradle](https://docs.gradle.org/4.6/userguide/installation.html)。

### 第 7 步：将 Gradle 包装器添加到项目

在项目根目录下的 shell 或命令行中键入：

```Shell
gradle wrapper
```

### 步骤 8：生成并运行示例

在项目根目录下的 shell 或命令行中键入：

```Shell
gradle run
```

此时将运行示例。

### 运行示例

命令行界面将在 Azure Active Directory 授权终结点上打开浏览器窗口。输入你的用户名和密码以进行身份验证。通过身份验证后，你将进入示例应用的授权窗口。查看并接受示例应用请求的作用域。单击授权窗口上的“确定”按钮。浏览器将导航到 `https://login.microsoftonline.com`。复制完整的重定向 URL 地址并将其粘贴到文本编辑器中。它应类似于以下 url：

```http
https://login.microsoftonline.com/common/oauth2/nativeclient?code={IAQABAAIAAABHh4kmS_aKT5XrjzxRAtHz5S...p7OoAFPmGPqIq-1_bMCAA}&session_state=dd64ce71-4424-494b-8818-be9a99ca0798
```

> **注意：**为清楚起见，已截断 URL 示例。已在示例中添加 `{` and `}`，以出于说明目的分隔授权代码。

授权代码在 `?code=` 之后开始，并在 `&session_state` 之前结束。

将授权代码复制到系统剪贴板，并将其粘贴到控制台的“`在此处粘贴授权代码`”下方。

系统将提示：

```Shell
Hello, {your name}. Would you like to send an email to yourself or someone else?
Enter the address to which you'd like to send a message. If you enter nothing, the message will go to your address
```

输入电子邮件地址或将提示留空后，将向你或在控制台中输入的地址发送电子邮件。

你可以向其他地址发送电子邮件，方法是对提示“`是否要发送其他邮件？”回答“y”（是）键入“y”（是）和其他任何键退出。`
