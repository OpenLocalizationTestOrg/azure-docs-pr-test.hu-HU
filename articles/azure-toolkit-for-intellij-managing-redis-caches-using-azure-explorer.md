---
title: "Redis Cache-gyorsítótárak az intellij-t Azure Explorerrel kezelése |} Microsoft Docs"
description: "Útmutató az Azure redis gyorsítótár felügyelni az intellij-t az Azure-kezelővel használatával."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/14/2017
ms.author: robmcm
ms.openlocfilehash: 9ab8ae17ee2a92b5b16d2210366c00b5b8023fa8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a>Redis Cache-gyorsítótárak az intellij-t Azure Explorerrel kezelése

Az Azure-kezelővel, amely IntelliJ Azure eszköztára része biztosít a Java fejlesztők egy könnyen használható megoldást a redis-gyorsítótárak a Azure fiók belül az IntelliJ IDE.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a>A Redis Cache létrehozása az IntelliJ használatával

A következő lépések végigvezetik Önt az Azure-kezelővel redis gyorsítótár létrehozásához szükséges lépéseket.

1. A lépések az Azure-fiókjával jelentkezzen be a [bejelentkezési az utasítások az intellij-t Azure eszköztára] cikk.

1. Az a **Azure Explorer** ablak eszköze, bontsa ki a **Azure** csomópontot, kattintson a jobb gombbal **Redis Cache-gyorsítótárak**, és kattintson a **Redis Cache létrehozása**.

   ![Redis gyorsítótár menü létrehozása][CR01]

1. Ha a **új Redis Cache** párbeszédpanel, adja meg a következő beállításokat:

   ![Hozzon létre új Redis Cache párbeszédpanel][CR02]

   a. **DNS-név**: Adja meg a DNS altartományában a új redis gyorsítótár, amely a rendszer $a ". redis.cache.windows.net"; például: *wingtiptoys.redis.cache.windows.net*.

   b. **Előfizetés**: Adja meg az új redis gyorsítótár használni kívánt Azure-előfizetést.

   c. **Erőforráscsoport**: Adja meg az erőforráscsoport a redis gyorsítótár; meg kell adnia az alábbi lehetőségek közül:
      * **Hozzon létre új**: megadhatja, hogy kívánja-e egy új erőforráscsoport létrehozásához.
      * **Használja a már meglévő**: meghatározza a fog választani az erőforráscsoportok az Azure-fiókjával társított listájából.

   d. **Hely**: Adja meg a helyet, ahol a redis gyorsítótárat létrehozni; például *USA nyugati régiója*.

   e. **IP-címek**: Adja meg a redis gyorsítótárat használ tarifacsomagtól; Ez a beállítás meghatározza, hogy az ügyfélkapcsolatok számát. (További információkért lásd: [Redis gyorsítótár árképzési].)

   f. **A nem SSL port**: Megadja, hogy a redis gyorsítótár lehetővé teszi, hogy az SSL kapcsolatok; alapértelmezés szerint csak az SSL-kapcsolatok engedélyezett.

1. A redis gyorsítótár beállításainak megadását, kattintson a **OK**.

A redis gyorsítótár létrehozása után megjelenik az Azure-kezelővel történik.

   ![Redis gyorsítótár Azure Explorerben][CR03]

> [!NOTE]
>
> Az Azure konfigurálásával kapcsolatos további információkat a redis gyorsítótár-beállítások című [konfigurálása az Azure Redis Cache].
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a>A Redis Cache-ben az IntelliJ tulajdonságainak megjelenítése

1. Az Azure-kezelővel, kattintson a jobb gombbal a redis gyorsítótárt, és kattintson a **tulajdonságainak megjelenítése**.

   ![Az Azure Explorer helyi menü megjelenítése a redis gyorsítótár tulajdonságai][SP01]

1. Az Azure-kezelővel a redis gyorsítótárat a Tulajdonságok megjelenítése.

   ![A Redis Cache-gyorsítótár tulajdonságai][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a>Törölje a Redis Cache IntelliJ használatával

1. Az Azure-kezelővel, kattintson a jobb gombbal a redis gyorsítótárt, és kattintson a **törlése**.

   ![A redis gyorsítótár törléséhez az Azure Explorer helyi menü][DE01]

1. Kattintson a **Igen** amikor a rendszer kéri a redis gyorsítótár törléséhez.

   ![Redis gyorsítótár kérdés törlése][DE02]

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

Azure redis gyorsítótár, a konfigurációs beállításokat és a díjszabás kapcsolatos további információkért tekintse meg a következőket:

* [Azure Redis Cache]
* [Redis gyorsítótár dokumentáció]
* [Redis gyorsítótár árképzési]
* [konfigurálása az Azure Redis Cache]

<!-- URL List -->

[Redis gyorsítótár árképzési]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Redis gyorsítótár dokumentáció]: ./redis-cache/index.md
[konfigurálása az Azure Redis Cache]: ./redis-cache/cache-configure.md
[bejelentkezési az utasítások az intellij-t Azure eszköztára]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
