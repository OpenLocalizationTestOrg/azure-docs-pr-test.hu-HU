---
title: "Tábla API aaaAzure Cosmos DB globális terjesztési oktatóanyaga |} Microsoft Docs"
description: "Ismerje meg, hogyan hello tábla API toosetup Azure Cosmos DB globális terjesztési használatával."
services: cosmos-db
keywords: "globális terjesztési, tábla"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: d2a995e09c37f9449856aef2ab707e95eb8a540c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-table-api"></a>Hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello tábla API

Ez a cikk megmutatjuk, hogyan toouse hello Azure portál toosetup Azure Cosmos DB globális terjesztési, és csatlakoztassa a hello tábla API (előzetes verzió) használatával.

Ez a cikk ismerteti a következő feladatok hello: 

> [!div class="checklist"]
> * Konfigurálja a globális terjesztési hello Azure-portál használatával
> * Globális terjesztési használatával hello konfigurálása [tábla API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-table-api"></a>Csatlakozás előnyben részesített régióba tooa hello tábla API

A rendelés tootake előnyeit [globális terjesztési](distribute-data-globally.md), ügyfélalkalmazások hello rendezett használt tooperform dokumentum műveletek régiók toobe preferencia listája adhat meg. Ezt úgy teheti hello beállítása `TablePreferredLocations` hello alkalmazások konfigurációja az Azure Storage szolgáltatás SDK hello előzetes konfigurációs értéket. Hello Azure Cosmos DB fiók konfigurációtól függően aktuális területi rendelkezésre állási és hello beállításokat szabályozó lista megadva, a legtöbb optimális végpont választja ki hello Azure Storage szolgáltatás SDK tooperform hello írási és olvasási műveletek.

Hello `TablePreferredLocations` olvasása előnyben részesített (többhelyű) helyek vesszővel tagolt listáját kell tartalmaznia. Minden ügyfél példány előnyben részesített hello ahhoz, hogy kis késleltetésű olvasási adhatja meg e régiók egy részét. hello régiók névvel kell ellátni használatával a [megjelenített neveket](https://msdn.microsoft.com/library/azure/gg441293.aspx), például `West US`.

Az összes olvasási küldi el toohello első rendelkezésre álló terület hello `TablePreferredLocations` listája. Hello kérelem sikertelen lesz, ha hello ügyfél le hello lista toohello következő terület sikertelen, és így tovább.

hello SDK csak kísérli meg a megadott hello régiók tooread `TablePreferredLocations`. Így például ha hello adatbázisfiók három régiókban, de hello csak ügyfélszámítógép két hello nem írási régiók `TablePreferredLocations`, majd nincs olvasási szolgáltató hello írási régió, még akkor is, a feladatátvétel hello eset kívül.

hello SDK automatikusan elküld minden írások toohello aktuális írási terület.

Ha hello `TablePreferredLocations` tulajdonsága nincs beállítva, az összes kérelem szolgáltató hello aktuális írási régióban.

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

Ez azt, hogy ez az oktatóanyag befejezése. Megismerheti, hogyan toomanage hello olvasásával globálisan replikált fiókja konzisztencia [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md). És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Konfigurálja a globális terjesztési hello Azure-portál használatával
> * Globális terjesztési hello DocumentDB API-k használatával konfigurálása

Most már folytathatja a következő útmutató toolearn toohello hogyan helyileg toodevelop hello Azure Cosmos DB helyi emulátor.

> [!div class="nextstepaction"]
> [Hello emulátorral helyileg fejlesztése](local-emulator.md)
