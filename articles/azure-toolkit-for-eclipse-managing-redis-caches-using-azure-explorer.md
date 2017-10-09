---
title: "Azure-kezelőjét az Eclipse aaaManaging Redis Cache-gyorsítótárak használatával hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure redis gyorsítótárazza az Eclipse hello Azure Explorer használatával."
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
ms.openlocfilehash: aa0c38862bda7919a3fc6c53c2fdaf555dd64bff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-redis-caches-using-hello-azure-explorer-for-eclipse"></a>Redis Cache-gyorsítótárak hello Azure Explorer használata az Eclipse kezelése

hello Azure-kezelővel, amely hello Azure eszköztára Eclipse része, biztosít a Java-fejlesztők egy könnyen kezelhető megoldás az kezelése redis gyorsítótár belül a Azure fiók hello Eclipse IDE.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-eclipse"></a>Hozzon létre egy Redis gyorsítótárhoz eclipse-ben

hello lépések végigvezetik Önt hello lépéseket toocreate a redis gyorsítótár hello Azure Explorer használatával.

1. Jelentkezzen be tooyour hello lépésekkel hello Azure-fiók [bejelentkezési az utasítások hello Azure eszköztára Eclipse] cikk.

1. A hello **Azure Explorer** ablak eszköze, bontsa ki a hello **Azure** csomópontot, kattintson a jobb gombbal **Redis Cache-gyorsítótárak**, és kattintson a **Redis Cache létrehozása**.

   ![Redis gyorsítótár menü létrehozása][CR01]

1. Ha hello **új Redis Cache** párbeszédpanel, adja meg az alábbi beállítások hello:

   ![Hozzon létre új Redis gyorsítótár párbeszédpanel][CR02]

   a. **DNS-név**: Adja meg a DNS altartományában hello hello új redis gyorsítótárat, amely túl van $a a ". redis.cache.windows .net"; például: *wingtiptoys.redis.cache.windows.net*.

   b. **Előfizetés**: hello toouse hello új redis gyorsítótár kívánt Azure-előfizetés határozza meg.

   c. **Erőforráscsoport**: Adja meg a redis gyorsítótár hello erőforráscsoport; toochoose az alábbi beállítások hello van szüksége:
      * **Hozzon létre új**: megadhatja, hogy kívánja-e toocreate egy új erőforráscsoportot.
      * **Használja a már meglévő**: meghatározza a fog választani az erőforráscsoportok az Azure-fiókjával társított listájából.

   d. **Hely**: a redis gyorsítótár létrehozási helyének; hello helyét adja meg például *USA nyugati régiója*.

   e. **IP-címek**: Adja meg a redis gyorsítótárat használ tarifacsomagtól; Ez a beállítás határozza meg, hogy hello ügyfélkapcsolatokat. (További információkért lásd: [Redis gyorsítótár árképzési].)

   f. **A nem SSL port**: Megadja, hogy a redis gyorsítótár lehetővé teszi, hogy az SSL kapcsolatok; alapértelmezés szerint csak az SSL-kapcsolatok engedélyezett.

1. A redis gyorsítótár beállításainak megadását, kattintson a **OK**.

A redis gyorsítótár létrehozása után megjelenő hello Azure-kezelővel történik.

   ![Redis gyorsítótár Azure Explorerben][CR03]

> [!NOTE]
>
> Az Azure konfigurálásával kapcsolatos további információkat a redis gyorsítótár-beállítások című [hogyan tooconfigure Azure Redis Cache].
>

## <a name="display-hello-properties-for-your-redis-cache-in-eclipse"></a>Az eclipse-ben a Redis gyorsítótár hello tulajdonságok megjelenítése

1. A hello Azure-kezelővel, kattintson a jobb gombbal a redis gyorsítótárt, és kattintson a **tulajdonságainak megjelenítése**.

   ![Az Azure Explorer helyi menü toodisplay tulajdonságai egy redis gyorsítótárhoz][SP01]

1. hello Azure-kezelővel a redis gyorsítótár hello tulajdonságainak megjelenítése.

   ![A Redis Cache-gyorsítótár tulajdonságai][SP02]

## <a name="delete-your-redis-cache-by-using-eclipse"></a>Törölje a Redis Cache Eclipse használatával

1. A hello Azure-kezelővel, kattintson a jobb gombbal a redis gyorsítótárt, és kattintson a **törlése**.

   ![Az Azure Explorer helyi menü toodelete a redis gyorsítótár][DE01]

1. Kattintson a **OK** amikor toodelete a redis gyorsítótár kéri.

   ![Redis gyorsítótár kérdés törlése][DE02]

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

Azure redis gyorsítótár, a konfigurációs beállításokat és a díjszabás kapcsolatos további információkért tekintse meg a következő hivatkozások hello:

* [Azure Redis Cache]
* [Redis gyorsítótár dokumentáció]
* [Redis gyorsítótár árképzési]
* [hogyan tooconfigure Azure Redis Cache]

<!-- URL List -->

[Redis gyorsítótár árképzési]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Redis gyorsítótár dokumentáció]: ./redis-cache/index.md
[hogyan tooconfigure Azure Redis Cache]: ./redis-cache/cache-configure.md
[bejelentkezési az utasítások hello Azure eszköztára Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE02.png
