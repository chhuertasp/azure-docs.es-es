---
title: "Instalación de Visual Studio y SSDT para SQL Data Warehouse | Microsoft Docs"
description: "Instalación de herramientas de desarrollo de Visual Studio y SQL Server Data Tools (SSDT) para Almacenamiento de datos SQL de Azure"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 03/30/2017
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: ae025ee2e6865b225efc5e225e261ac579a339aa
ms.lasthandoff: 04/03/2017


---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Instalación de Visual Studio y SSDT para SQL Data Warehouse
Si desea desarrollar aplicaciones para SQL Data Warehouse, se recomienda usar la versión más reciente de Visual Studio, junto con la última versión de SQL Server Data Tools (SSDT).  También se admite Visual Studio 2013 Update 5 con SSDT por compatibilidad con versiones anteriores.  

Con Visual Studio con SSDT, podrá usar el Explorador de objetos de SQL Server para explorar visualmente tablas, vistas, procedimientos almacenados y muchos más objetos en su instancia de Almacenamiento de datos SQL, así como para ejecutar consultas.

> [!NOTE]
> Almacenamiento de datos SQL no es compatible aún con proyectos de base de datos de Visual Studio.  Esta característica se agregará en una versión futura.
> 
> 

## <a name="step-1-install-visual-studio"></a>Paso 1: Instalación de Visual Studio
Siga estos vínculos para descargar e instalar Visual Studio. Si ya tiene Visual Studio 2013 o una versión posterior instalada, vaya directamente al paso 2 para instalar SSDT.

1. [Descargue Visual Studio][].
2. Siga la guía [Instalar Visual Studio][Installing Visual Studio] en MSDN y elija las configuraciones predeterminadas.

## <a name="step-2-install-ssdt"></a>Paso 2: Instalación de SSDT
Para instalar SSDT para Visual Studio, simplemente busque una actualización de SSDT desde dentro de Visual Studio siguiendo estos pasos.

1. En Visual Studio, haga clic en **Herramientas** / **Extensiones y actualizaciones...** / **Actualizaciones**.
2. Seleccione **Actualizaciones de productos** y busque la actualización **Microsoft SQL Server Update para herramientas de base de datos**.

Si no hay ninguna actualización, debería tener la última versión instalada.  Para confirmar que SSDT está instalado, haga clic en **Ayuda** / **Acerca de Microsoft Visual Studio** y busque SQL Server Data Tools en la lista.  La versión más reciente de SSDT es 14.0.60525.0.  Si la opción para instalar no está disponible en Visual Studio, también puede visitar la página [Descargar las últimas herramientas de datos SQL Server][SSDT Download] para descargar e instalar SSDT manualmente.

## <a name="next-steps"></a>Pasos siguientes
Ahora que tiene la versión más reciente de SSDT, ya puede [conectarse][connect] a su instancia de Almacenamiento de datos SQL.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Descargue Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx

