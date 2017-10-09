---
title: "aaaInstall Visual Studio és az SSDT az SQL Data Warehouse |} Microsoft Docs"
description: "A Visual Studio és az SQL Server Development Tools (SSDT) telepítése az Azure SQL Data Warehouse-hoz"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
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
ms.author: anvang;barbkess
ms.openlocfilehash: cf49c13d5cab598ed127f5702c04168b62ede0dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>A Visual Studio és az SSDT telepítése SQL Data Warehouse
az SQL Data Warehouse alkalmazások toodevelop, azt javasoljuk, a Visual Studio legújabb verziójának hello hello legújabb verziójú SQL Server Data Tools (SSDT).  A Visual Studio 2013 Update 5 és az SSDT együttes használata is támogatott a visszamenőleges kompatibilitás érdekében.  

Visual Studio és az SSDT együttes használata lehetővé teszi a toouse hello SQL Server Object Explorer toovisually megismerkedhet a táblákat, nézeteket, tárolt eljárásokat és sok más objektumot az SQL Data Warehouse, valamint a lekérdezések futtatása.

> [!NOTE]
> Az SQL Data Warehouse még nem támogatja a Visual Studio-adatbázisprojekteket.  Ez a szolgáltatás egy későbbi verzióban lesz elérhető.
> 
> 

## <a name="step-1-install-visual-studio"></a>1. lépés: A Visual Studio telepítése
Kövesse az alábbi hivatkozások toodownload, és telepítse a Visual Studio. Ha már rendelkezik a Visual Studio 2013 vagy újabb verziója, kihagyhatja tooStep 2, az SSDT telepítése.

1. [Töltse le a Visual Studio][].
2. Hajtsa végre a hello [Visual Studio telepítése] [ Installing Visual Studio] útmutatót az MSDN Webhelyén, és válassza ki a hello alapértelmezett konfigurációkat.

## <a name="step-2-install-ssdt"></a>2. lépés: Az SSDT telepítése
az SSDT a Visual Studio tooinstall egyszerűen keressen a Visual studióban az SSDT-frissítéseket az alábbiak szerint.

1. A Visual Studióban kattintson a **eszközök** / **bővítmények és frissítések...** / **Frissítések**
2. Válassza a **Product Updates** (Termékfrissítések) lehetőséget, majd keresse a **Microsoft SQL Server Update for database tooling** (Microsoft SQL Server frissítése adatbáziseszközökkel) elemet

Ha a frissítés nem található, akkor szüksége lesz hello legfrissebb verzió van telepítve.  tooconfirm az SSDT telepítve van, kattintson a **súgó** / **kapcsolatban a Microsoft Visual Studio** és hello listában keresse meg az SQL Server Data Tools összetevővel.  hello SSDT legújabb verziója a 14.0.60525.0-s verzió.  Ha hello beállítás tooinstall nem érhető el a Visual Studióból, ellátogathat hello [SSDT letöltése] [ SSDT Download] toodownload lapon, és az SSDT manuális telepítése.

## <a name="next-steps"></a>Következő lépések
Most, hogy hello SSDT legújabb verzióját, készen áll túl[csatlakozás] [ connect] tooyour SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Töltse le a Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
