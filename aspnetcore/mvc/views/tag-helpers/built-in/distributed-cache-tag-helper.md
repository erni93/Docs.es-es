---
title: Aplicación auxiliar de etiquetas de caché distribuida en ASP.NET Core
author: pkellner
description: Muestra cómo trabajar con la aplicación auxiliar de etiqueta de caché
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a35a5795c086273e773c613c483fc6343c694bf2
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342177"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Aplicación auxiliar de etiquetas de caché distribuida en ASP.NET Core

Por [Peter Kellner](http://peterkellner.net) 

La aplicación auxiliar de etiquetas de caché distribuida proporciona la capacidad de mejorar drásticamente el rendimiento de la aplicación ASP.NET Core al permitir almacenar en caché su contenido en un origen de caché distribuida.

La aplicación auxiliar de etiquetas de caché distribuida hereda de la misma clase base que la aplicación auxiliar de etiquetas de caché. Todos los atributos asociados a la aplicación auxiliar de etiquetas de caché también funcionarán en la aplicación auxiliar de etiquetas de caché distribuida.

La aplicación auxiliar de etiquetas de caché distribuida sigue el **principio de dependencias explícitas** conocido como **inserción de constructores**. En concreto, el contenedor de interfaz `IDistributedCache` se pasa al constructor de la aplicación auxiliar de etiquetas de caché distribuida. Si no se ha creado ninguna implementación específica de `IDistributedCache` en `ConfigureServices`, que normalmente se encuentra en startup.cs, la aplicación auxiliar de etiquetas de caché distribuida usará el mismo proveedor en memoria para almacenar datos en caché que la aplicación auxiliar de etiquetas de caché básica.

## <a name="distributed-cache-tag-helper-attributes"></a>Atributos de la aplicación auxiliar de etiquetas de caché distribuida

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority

Vea la aplicación auxiliar de etiquetas de caché para obtener las definiciones. La aplicación auxiliar de etiquetas de caché distribuida hereda de la misma clase que la aplicación auxiliar de etiquetas de caché, de modo que todos estos atributos son iguales que los de la aplicación auxiliar de etiquetas de caché.

- - -

### <a name="name-required"></a>Atributo name (obligatorio)

| Tipo de atributo    | Valor de ejemplo     |
|----------------   |----------------   |
| cadena    | "my-distributed-cache-unique-key-101"     |

El atributo `name` obligatorio se usa como clave de la caché almacenada para cada instancia de una aplicación auxiliar de etiquetas de caché distribuida. A diferencia de la aplicación auxiliar de etiquetas de caché básica, que asigna una clave a cada instancia de la aplicación auxiliar de etiquetas de caché en función del nombre de la página de Razor y la ubicación de la aplicación auxiliar de etiquetas en la página de Razor, la aplicación auxiliar de etiquetas de caché distribuida solo basa su clave en el atributo `name`.

Ejemplo de uso:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implementaciones de IDistributedCache de la aplicación auxiliar de etiquetas de caché distribuida

Hay dos implementaciones de `IDistributedCache` integradas en ASP.NET Core. Una se basa en SQL Server y la otra, en Redis. En <xref:performance/caching/distributed> encontrará detalles de estas implementaciones. Ambas implementaciones requieren establecer una instancia de `IDistributedCache` en el archivo *Startup.cs* de ASP.NET Core.

No hay atributos de etiqueta asociados específicamente con el uso de implementaciones concretas de `IDistributedCache`.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
