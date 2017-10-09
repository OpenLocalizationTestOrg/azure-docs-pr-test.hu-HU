---
title: "Azure Search szolgáltatás REST API verziója 2016-09-01 aaaUpgrading toohello |} Microsoft Docs"
description: "Azure Search szolgáltatás REST API verziója 2016-09-01 toohello frissítése"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 6183fa6c-48bb-4af7-adae-4be3bc43c3ed
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: brjohnst
ms.openlocfilehash: d0276b9cc52996a59f9aa726c27e62c6082eb908
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrading-toohello-azure-search-service-rest-api-version-2016-09-01"></a>Azure Search szolgáltatás REST API verziója 2016-09-01 toohello frissítése
2015-02-28 verzió vagy 2015-02-28-előzetes verziója hello használata [Azure Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), ez a cikk segít frissíteni alkalmazás toouse hello következő általánosan elérhető API-verzióval, 2016-09-01.

Hello REST API verziója 2016-09-01 korábbi verzióihoz képest végrehajtott egyes módosításokat tartalmazza. Ezek a legtöbbször visszamenőlegesen kompatibilis, így a kód módosítása csak csatlakozhatnak, attól függően, hogy milyen verziójú használata előtt érdemes beállítani. Lásd: [lépéseket tooupgrade](#UpgradeSteps) útmutatást toochange kód toouse hello új API-verzióval.

> [!NOTE]
> Az Azure Search szolgáltatás példányát támogatja több REST API-t, többek között a legújabb hello. Egy verzió toouse tovább már nem hello legújabb, de azt javasoljuk, hogy áttelepítette-e a kód toouse hello legújabb verziója.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a>2016-09-01 verzió újdonságai
Verzió 2016-09-01 hello második általánosan elérhető kiadása hello Azure Search szolgáltatás REST API. Az API-verzió az új funkciói a következők:

* [Egyéni lekérdezések](https://aka.ms/customanalyzers), amely hello folyamat alakításával előállított szöveges példánycímkének és kereshető jogkivonatokba tootake ellenőrzését lehetővé teszik.
* [Az Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) és [Azure Table Storage](search-howto-indexing-azure-tables.md) indexelők, amelyek lehetővé teszik tooeasily adatok importálása az Azure storage Azure Search szolgáltatásba történő ütemezés vagy igény szerinti.
* [Hozzárendelések mezőben](search-indexer-field-mappings.md), amelyek lehetővé teszik, hogy toocustomize hogyan indexelők adatok importálása az Azure Search.
* ETag-EK, amelyek lehetővé teszik egy feldolgozási szálbiztos módon tooupdate hello definíciók indexek, az indexelők és az adatforrások. 

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a>Lépéseket tooupgrade
Ha 2015-02-28 verzióról frissít, valószínűleg nem kell toomake bármely módosítások tooyour kód toochange hello verziószáma eltérő. hello csak helyzetekben, ahol szükség lehet a toochange kódot, amikor:

* A kód sikertelen lesz, amikor egy API-válaszból ismeretlen tulajdonságok vissza. Alapértelmezés szerint az alkalmazás figyelmen kívül hagyja-tulajdonságok nem értelmezi.
* A kód API-kérelmek tartósan fennáll, és megpróbál tooresend őket toohello új API-verzió. Például ez előfordulhat, hogy fordulhat elő, ha az alkalmazás továbbra is fennáll, hello Search API által visszaadott folytatási jogkivonatok (további információkért tekintse meg `@search.nextPageParameters` a hello [keresési API-referencia](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).

Ha bármelyik ilyen helyzetekben tooyou, majd szükség lehet toochange a kód ennek megfelelően. Ellenkező esetben nem végez módosítást kell szükséges kivéve, ha azt szeretné, hogy hello segítségével toostart [új szolgáltatások](#WhatsNew) verzió 2016-09-01.

Ha verzió 2015-02-28-Preview frissít, a fenti hello is vonatkozik, de is figyelembe kell venni, hogy bizonyos előzetes verziójú funkciók nem érhetők el verziójában 2016-09-01:

* Az Azure Blob Storage indexelő támogatása CSV-fájlok és a JSON-tömbök tartalmazó BLOB.
* Szinonimák
* "Több a" lekérdezések

Ha a kódot használja ezeket a szolgáltatásokat, nem fogja az használatának egyik eltávolítása nélkül too2016-09-01 képes tooupgrade.

> [!IMPORTANT]
> Adjon ne feledje, az előzetes API-k tesztelésében és értékelésében készült, és nem használható üzemi környezetben.
> 
> 

## <a name="conclusion"></a>Összegzés
Ha további részleteket a hello Azure Search szolgáltatás REST API használatával van szüksége, tekintse meg a nemrég frissített hello [API-referencia](https://msdn.microsoft.com/library/azure/dn798935.aspx) az MSDN Webhelyén.

Azure Search szívesen fogadjuk a visszajelzéseket. Ha problémákat tapasztal, érzi, hogy szabad tooask nekünk a Súgó a hello [Azure Search MSDN fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) vagy [StackOverflow](http://stackoverflow.com/). Ha most kérni a kérdés kapcsolatos Azure keresése StackOverflow, győződjön meg arról, hogy tootag együtt `azure-search`.

Köszönjük, hogy az Azure Search használatával!

