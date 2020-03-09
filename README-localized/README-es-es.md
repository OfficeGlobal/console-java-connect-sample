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
# Integrar la API de Microsoft Graph en una aplicación de línea de comandos de Java con un nombre de usuario y una contraseña

Este ejemplo muestra una aplicación de línea de comandos que llama a la API de Microsoft Graph protegida por Azure AD. La aplicación usa la [biblioteca de autenticación ScribeJava](https://github.com/scribejava/scribejava) para obtener un token Web de JSON (JWT) a través del protocolo OAuth 2.0. El JWT devuelto se denomina **token de acceso**. El token de acceso se agregará a todas las solicitudes de la API de Microsoft Graph como encabezados HTTP. Se autentica al usuario y obtiene acceso al servicio.

Este ejemplo muestra cómo usar **ScribeJava** para autenticar a los usuarios a través de credenciales simples (nombre de usuario y contraseña) con una interfaz de solo texto.

## Inicio rápido

Empezar a usar el ejemplo es fácil. Se configuró para funcionar con la configuración mínima predeterminada.

### Paso 1: Registro de un espacio empresarial de Azure AD

Para usar este ejemplo, se necesita un espacio empresarial de Azure Active Directory. Si no está seguro de qué es un espacio empresarial o de cómo obtendría uno, lea [¿qué es un espacio empresarial de Azure AD?](http://technet.microsoft.com/library/jj573650.aspx) o [registrarse en Azure como organización](http://azure.microsoft.com/documentation/articles/sign-up-organization/). Estos documentos deberían ayudarle a empezar a usar Azure AD.

### Paso 2: Descargue Java (7 y superior) para su plataforma

Para usar este ejemplo, se necesita una instalación en funcionamiento de [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html) y [Gradle](https://gradle.org/).

El ejemplo se ha generado con el complemento Gradle para IntelliJ IDEA IDE. El archivo de proyecto build.gradle se configura para permitirle ejecutar el ejemplo desde IntelliJ. La configuración de la compilación de ejecución permite ejecutar o depurar el ejemplo.

### Paso 3: Descargar la aplicación de ejemplo y los módulos

Luego, clone el repositorio de ejemplo e instale las dependencias del proyecto.

Desde la línea de comandos o shell:

* `$ git@github.com:microsoftgraph/console-java-connect-sample.git`
* `$ cd console-java-connect-sample`

### Paso 4: Registrar la aplicación de la consola de Java Connect

1. Inicie sesión en el [Azure Portal: Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) mediante su cuenta personal, profesional o escolar.

2. Seleccione **Nuevo registro**.

3. En la sección **Nombre**, introduzca un nombre de aplicación significativo que se le mostrará a los usuarios de la aplicación

1. En la sección **Tipos de cuentas admitidas**, seleccione **Cuentas en cualquier directorio organizacional y cuentas personales de Microsoft (ejemplo, Skype, Xbox, Outlook.com)**  

1. Seleccione **Registrar** para crear la aplicación. 
	
   En la página de información general de la aplicación se muestran las propiedades de la aplicación.

4. Copie la **Identificación del cliente de aplicación**. Este es el identificador único de su aplicación. 

1. En la lista de páginas de la aplicación, seleccione **autenticación**.

1. En **redirigir URI** en la sección **URI de redirección sugerida para clientes públicos (móvil, escritorio)**, active la casilla junto a **https://login.microsoftonline.com/common/oauth2/nativeclient**

8. Elija **Guardar**.

> **Nota:** El punto de conexión de autorización de Azure Active Directory v2.0 usa [el consentimiento incremental y el dinámico](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) lo que significa que no es necesario establecer permisos (que ahora se denominan **ámbitos**) cuando se registra la aplicación. La aplicación pide ámbitos en tiempo de ejecución.

### Paso 5: Configure su aplicación mediante Constants.java

Con su editor de código favorito, abra **..\\console-java-connect-sample\\src\\main\\java\\com\\microsoft\\graphsample\\connect\\Constants.java**. Pegue el ID. de aplicación del portapapeles en Constants.java, línea 11 para reemplazar `ENTER_YOUR_CLIENT_ID`.

```java
public class Constants {

    public final static String CLIENT_ID = "ENTER_YOUR_CLIENT_ID";

//...
}
```

### Paso 6: Instalar Gradle

Si no tiene instalado el sistema de compilación Gradle, [instale Gradle](https://docs.gradle.org/4.6/userguide/installation.html).

### Paso 7: Agregar el contenedor Gradle al proyecto

Desde la línea de comandos o shell en la raíz del proyecto:

```Shell
gradle wrapper
```

### Paso 8: Compilar y ejecutar el ejemplo

Desde la línea de comandos o shell en la raíz del proyecto:

```Shell
gradle run
```

Se ejecutará el ejemplo.

### Ejecutar el ejemplo

La interfaz de línea de comandos abre una ventana del explorador en el punto de conexión de autorización de Azure Active Directory. Escriba su nombre de usuario y contraseña para autenticar. Cuando se le autentique, se le dirigirá a una ventana de autorización para la aplicación de muestra. Revise y acepte los ámbitos solicitados por la aplicación de ejemplo. Haga clic en el botón Aceptar en la ventana Authorization. El explorador se desplaza a `https://login.microsoftonline.com`. Copie la dirección URL de redireccionamiento completa y péguela en un editor de texto. Debería ser similar a la siguiente:

```http
https://login.microsoftonline.com/common/oauth2/nativeclient?code={IAQABAAIAAABHh4kmS_aKT5XrjzxRAtHz5S...p7OoAFPmGPqIq-1_bMCAA}&session_state=dd64ce71-4424-494b-8818-be9a99ca0798
```

> **Nota:** La URL de ejemplo está truncada para mayor claridad. `{` y `}` se han agregado al ejemplo para delimitar el código de autorización para fines ilustrativos.

El código de autorización se inicia después de `?code=` y acaba antes de `&session_state`.

Copie el código de autorización en el portapapeles del sistema y péguelo en la consola, en `Y pegue el código de autorización aquí`.

Se le preguntará:

```Shell
Hello, {your name}. Would you like to send an email to yourself or someone else?
Enter the address to which you'd like to send a message. If you enter nothing, the message will go to your address
```

Después de escribir una dirección de correo electrónico o dejar la solicitud en blanco, se le enviará un correo electrónico o a la dirección que escriba en la consola.

Puede enviar un correo electrónico a otras direcciones si responde con una "y" a la pregunta `quiere enviar otro mensaje? Escriba "y" para sí y cualquier otra tecla para salir.`
