---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Descubra los conceptos básicos para crear aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 68760f179c4d6e806510b727e2284f8c2c4a4ff6
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "41746093"
---
# <a name="aspnet-core-fundamentals"></a>Conceptos básicos de ASP.NET Core

Una aplicación de ASP.NET Core es una aplicación de consola que crea un servidor web en su método `Main`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

El método `Main` invoca [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), que sigue el [patrón del generador](https://wikipedia.org/wiki/Builder_pattern) para crear un host de web. El generador tiene métodos que definen el servidor web (por ejemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) y la clase de inicio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). En el ejemplo anterior, se asigna automáticamente el servidor web [Kestrel](xref:fundamentals/servers/kestrel). El host web de ASP.NET Core intenta ejecutarse en IIS, si está disponible. Otros servidores web, como [HTTP.sys](xref:fundamentals/servers/httpsys), se pueden usar al invocar el método de extensión adecuado. `UseStartup` se explica en la sección siguiente.

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, el tipo de valor devuelto de la invocación `WebHost.CreateDefaultBuilder`, proporciona muchos métodos opcionales. Algunos de estos métodos incluyen `UseHttpSys` para hospedar la aplicación en HTTP.sys y <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar el directorio de contenido raíz. Los métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> y <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> crean el objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

El método `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, que sigue el [patrón del generador](https://wikipedia.org/wiki/Builder_pattern) para crear un host de aplicación web. El generador tiene métodos que definen el servidor web (por ejemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) y la clase de inicio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). En el ejemplo anterior, se usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel). Si se invoca el método de extensión adecuado, se pueden usar otros servidores web, como [WebListener](xref:fundamentals/servers/weblistener). `UseStartup` se explica en la sección siguiente.

`WebHostBuilder` proporciona muchos métodos opcionales, incluido <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> para hospedar en IIS e IIS Express, y <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar el directorio de contenido raíz. Los métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> y <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> crean el objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.

::: moniker-end

## <a name="startup"></a>Inicio

El método `UseStartup` de `WebHostBuilder` especifica la clase `Startup` para la aplicación:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

La clase `Startup` es donde se define la canalización de control de solicitudes y donde se configuran los servicios necesarios para la aplicación. La clase `Startup` debe ser pública y contener los siguientes métodos:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> define los [servicios](#dependency-injection-services) que usa la aplicación (por ejemplo, ASP.NET Core MVC, Entity Framework Core, Identity). <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> define el [software intermedio](xref:fundamentals/middleware/index) al que se llama en la canalización de solicitudes.

Para obtener más información, vea <xref:fundamentals/startup>.

## <a name="content-root"></a>Raíz del contenido

La raíz del contenido es la ruta de acceso base a cualquier contenido que usa la aplicación, como [Razor Pages](xref:razor-pages/index), las vistas de MVC y los recursos estáticos. De forma predeterminada, la raíz del contenido es la misma ubicación que la ruta de acceso base de la aplicación para el archivo ejecutable que hospeda la aplicación.

## <a name="web-root"></a>Raíz web

La raíz web de una aplicación es el directorio del proyecto que contiene recursos públicos y estáticos, como archivos de imagen, CSS y JavaScript.

## <a name="dependency-injection-services"></a>Inserción de dependencias (servicios)

Un *servicio* es un componente que está pensado para su uso común en una aplicación. Los servicios se ponen a disposición a través de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection). ASP.NET Core incluye un contenedor de inversión del control (IoC) nativo que admite la [inserción de constructores](xref:mvc/controllers/dependency-injection#constructor-injection) de forma predeterminada. Si lo prefiere, puede reemplazar el contenedor predeterminado. Además de la [ventaja de acoplamiento flexible](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI hace que los servicios estén disponibles en toda la aplicación, por ejemplo, el [registro](xref:fundamentals/logging/index).

Para obtener más información, vea <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Software intermedio

En ASP.NET Core, se crea la canalización de solicitudes mediante [software intermedio](xref:fundamentals/middleware/index). El software intermedio de ASP.NET Core lleva a cabo las operaciones asincrónicas en un `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.

Normalmente, se agrega un componente de software intermedio denominado "XYZ" a la canalización al invocar a un método de extensión `UseXYZ` en el método `Configure`.

ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir su propio software intermedio personalizado. La [interfaz web abierta para .NET (OWIN)](xref:fundamentals/owin), que permite desacoplar las aplicaciones web de servidores web, es compatible con las aplicaciones de ASP.NET Core.

Para obtener más información, consulte <xref:fundamentals/middleware/index> y <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Inicio de solicitudes HTTP

<xref:System.Net.Http.IHttpClientFactory> está disponible para acceder a instancias de <xref:System.Net.Http.HttpClient> con el fin de realizar solicitudes HTTP.

Para obtener más información, vea <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Entornos

Los entornos, como *Desarrollo* y *Producción*, son un concepto de primera clase en ASP.NET Core y se pueden establecer mediante una variable de entorno, un archivo de configuración o un argumento de línea de comandos.

Para obtener más información, vea <xref:fundamentals/environments>.

## <a name="hosting"></a>Hospedaje

Las aplicaciones ASP.NET Core configuran e inician un *host*. Dicho host es responsable de la administración de inicio y duración de la aplicación.

Para obtener más información, vea <xref:fundamentals/host/index>.

## <a name="servers"></a>Servidores

El modelo de hospedaje de ASP.NET Core no escucha directamente las solicitudes. El modelo de hospedaje se basa en una implementación de servidor HTTP para reenviar la solicitud a la aplicación. La solicitud reenviada se empaqueta como un conjunto de objetos de característica al que se puede tener acceso a través de interfaces. ASP.NET Core incluye un servidor web administrado multiplataforma, denominado [Kestrel](xref:fundamentals/servers/kestrel). Kestrel suele ejecutarse tras un servidor web de producción, como [IIS](https://www.iis.net/) o [Nginx](http://nginx.org), en una configuración proxy invertida. Kestrel también puede ejecutarse como servidor perimetral expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.

Para obtener más información, vea <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Configuración

ASP.NET Core usa un modelo de configuración basado en pares de nombre-valor. El modelo de configuración no se basa en <xref:System.Configuration> o *web.config*. La configuración obtiene valores de un conjunto ordenado de proveedores de configuración. Los proveedores de configuración integrados admiten una gran variedad de formatos de archivo (XML, JSON, INI), variables de entorno y argumentos de línea de comandos. También puede escribir sus propios proveedores de configuración personalizados.

Para obtener más información, vea <xref:fundamentals/configuration/index>.

## <a name="logging"></a>Registro

ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro. Los proveedores integrados admiten el envío de registros a uno o varios destinos. Se pueden usar plataformas de registro de terceros.

Para obtener más información, vea <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Control de errores

ASP.NET Core tiene escenarios integrados para controlar los errores en las aplicaciones, incluidas una página de excepciones de desarrollador, páginas de errores personalizados, páginas de códigos de estado estáticos y control de excepciones de inicio.

Para obtener más información, vea <xref:fundamentals/error-handling>.

## <a name="routing"></a>Enrutamiento

ASP.NET Core ofrece casos para el enrutamiento de solicitudes de aplicación a los controladores de ruta.

Para obtener más información, vea <xref:fundamentals/routing>.

## <a name="file-providers"></a>Proveedores de archivos

ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos, lo que ofrece una interfaz común para trabajar con archivos entre plataformas.

Para obtener más información, vea <xref:fundamentals/file-providers>.

## <a name="static-files"></a>Archivos estáticos

El software intermedio de archivos estáticos trabaja con archivos estáticos, por ejemplo, HTML, CSS, imágenes y JavaScript.

Para obtener más información, vea <xref:fundamentals/static-files>.

## <a name="session-and-app-state"></a>Estado de sesión y aplicación

ASP.NET Core ofrece varios enfoques para conservar el estado de sesión y aplicación mientras el usuario examina una aplicación web.

Para obtener más información, vea <xref:fundamentals/app-state>.

## <a name="globalization-and-localization"></a>Globalización y localización

El hecho de crear un sitio web multilingüe con ASP.NET Core permite que este llegue a un público más amplio. Además, proporciona servicios y software intermedio para la localización de contenido en diferentes idiomas y referencias culturales.

Para obtener más información, vea <xref:fundamentals/localization>.

## <a name="request-features"></a>Características de la solicitud

Los detalles de implementación del servidor web relacionados con las solicitudes HTTP y las respuestas se definen en las interfaces. En las implementaciones del servidor web y el software intermedio se usan estas interfaces para crear y modificar la canalización de hospedaje de la aplicación.

Para obtener más información, vea <xref:fundamentals/request-features>.

## <a name="background-tasks"></a>Tareas en segundo plano

Las tareas en segundo plano se implementan como *servicios hospedados*. Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.

Para obtener más información, vea <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Acceso a HttpContext

`HttpContext` está disponible automáticamente al procesar las solicitudes con Razor Pages y MVC. En los casos en los que `HttpContext` no esté disponible, podrá acceder a `HttpContext` a través de la interfaz de <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> y su implementación predeterminada, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Para obtener más información, vea <xref:fundamentals/httpcontext>.

## <a name="websockets"></a>WebSockets

[WebSocket](https://wikipedia.org/wiki/WebSocket) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP. Se usa para aplicaciones de chat, tableros de cotizaciones, juegos y donde se necesite funcionalidad en tiempo real en una aplicación web. ASP.NET Core es compatible con casos de socket web.

Para obtener más información, vea <xref:fundamentals/websockets>.

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a>Metapaquete Microsoft.AspNetCore

El metapaquete [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) simplifica la administración de metapaquetes.

Para obtener más información, vea <xref:fundamentals/metapackage-app>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a>Metapaquete Microsoft.AspNetCore.All

El metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core incluye lo siguiente:

* Todos los paquetes admitidos por el equipo de ASP.NET Core.
* Todos los paquetes admitidos por Entity Framework Core.
* Dependencias internas y de terceros usadas por ASP.NET Core y Entity Framework Core.

Para obtener más información, vea <xref:fundamentals/metapackage>.

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>Entorno de ejecución de .NET Core frente a .NET Framework

Una aplicación de ASP.NET Core puede tener como destino el entorno de ejecución de .NET Core o .NET Framework.

Para más información, vea [Selección entre .NET Core y .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Elección entre ASP.NET Core y ASP.NET

Para obtener más información sobre cómo elegir entre ASP.NET Core y ASP.NET, vea <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.
