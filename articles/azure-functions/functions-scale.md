---
title: "Az Azure Functions használati és az App Service tervek |} Microsoft Docs"
description: "Ismerje meg, hogyan Azure Functions méretezi a eseményvezérelt munkaterhelések igényeinek."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "azure-függvények, függvények, eseményfeldolgozás, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/12/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70e77e09b2e2116153159167af61776398904a3c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-consumption-and-app-service-plans"></a>Az Azure Functions használati és az App Service tervek 

## <a name="introduction"></a>Bevezetés

Az Azure Functions két különböző módban is futtathatja: fogyasztás terv és az Azure App Service-csomag. A felhasználási terv automatikusan osztja ki a számítási teljesítményt, a kódja fut, kimenő terhelés kezelésére, szükség szerint arányosan, és majd arányosan csökken, amikor a kód nem fut. Igen nem kell a tétlen virtuális gépek után kell fizetnie, és nem kell előzetesen tartalékkapacitás. Ez a cikk foglalkozik a fogyasztás terven. Az App Service-csomag működésével kapcsolatos részletekért lásd: a [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Ha nem ismeri az Azure Functions, tekintse meg a [Azure Functions áttekintése](functions-overview.md).

Amikor létrehoz egy függvény alkalmazást, konfigurálnia kell az alkalmazást tartalmazó funkciók üzemeltetési terv. Mindkét üzemmódban, egy példányát a *Azure Functions állomás* feladatokat hajt végre. A terv vezérlők típusa:

* Hogyan állomás példányok is kiterjeszthető.
* Minden állomás számára elérhető erőforrások.

A terv típus jelenleg, a függvény alkalmazás létrehozásakor kell választania. Ezt követően nem módosítható. 

Az App Service-csomag a rétegek közötti méretezheti. A felhasználási terv Azure Functions automatikusan kezeli az összes erőforrás-elosztás.

## <a name="consumption-plan"></a>Használatalapú csomag

Amikor egy fogyasztás tervet használja, az Azure Functions állomás példányai dinamikusan felvétele, illetve eltávolítása a bejövő események száma alapján. Ez a csomag automatikusan méretezi, és van szó, a számítási erőforrások csak akkor, ha a függvények futnak. Felhasználás tervezze a függvény legfeljebb 10 perc futtathatja. 

> [!NOTE]
> Az alapértelmezett időtúllépési fogyasztás tervezze függvények érték 5 perc. Az érték növelhető 10 perc a függvény alkalmazás tulajdonságának módosításával `functionTimeout` a [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

A függvény alkalmazások minden funkciók között összesített értéket, és számlázási végrehajtási ideje és a használt memória alapul. További információkért lásd: a [árképzést ismertető oldalra Azure Functions].

A felhasználási tervben az alapértelmezett, és a következő előnyöket biztosítja:
- Után kell fizetnie, csak ha a függvények futnak.
- Horizontális felskálázás automatikusan, még nagy idején betölteni.

## <a name="app-service-plan"></a>App Service-csomag

Az App Service-csomag a függvény alkalmazások futtassa a Basic, Standard és Premium termékváltozat, a webalkalmazások hasonló dedikált virtuális gépeken. Dedikált virtuális gépek számára kiosztott az App Service-alkalmazásokhoz, ami azt jelenti, hogy a funkciók állomás mindig működik.

Fontolja meg az App Service-csomag a következő esetekben:
- Rendelkezik már meglévő, a túl többi App Service-példány már fut a virtuális gépek.
- A függvény alkalmazások futtatásához folyamatosan vagy majdnem folyamatosan várt.
- További CPU és memória beállítások, mint mi biztosítja a fogyasztás terven van szüksége.
- Hosszabb, mint a maximális engedélyezett a fogyasztás terv a végrehajtási idő futtatásához szükséges.

A virtuális gépek leválasztja a futásidejű és a memória méretét a költségek. Emiatt nem kell fizetnie több, mint a Virtuálisgép-példány lefoglalandó költségét. Az App Service-csomag működésével kapcsolatos részletekért lásd: a [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Az App Service-csomagot lehet manuálisan horizontálisan további Virtuálisgép-példányok hozzáadásával, vagy engedélyezheti az automatikus skálázási. További információkért lásd: [méretezése példányszám manuális vagy automatikus](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). Is költenie válasszon egy másik App Service-csomagra. További információkért lásd: [vertikális felskálázás az Azure alkalmazásban](../app-service-web/web-sites-scale.md). Ha azt tervezi, JavaScript-funkcióként futhat az App Service-csomagot, válasszon egy tervet, amely kevesebb magok rendelkezik. További információkért lásd: a [függvények JavaScript hivatkozás](functions-reference-node.md#choose-single-core-app-service-plans).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
###Always On

Ha az App Service-csomagot futtatja, engedélyeznie kell a **mindig a** beállítása, hogy a függvény alkalmazás megfelelő működését. Az App Service-csomagot a functions futtatókörnyezete kerül üresjárati tétlen, néhány perc múlva, csak a HTTP-eseményindítók fogja "felébreszteni" függvényeit. Ez hasonlít hogyan WebJobs rendelkeznie kell mindig engedélyezve van. 

Always On érhető csak az App Service-csomag. Felhasználás tervezze a platform aktiválja függvény alkalmazások automatikusan.

## <a name="storage-account-requirements"></a>Tárolási fiókra vonatkozó követelmények

A felhasználási terv vagy az App Service-csomag a függvény az alkalmazás csak a, amely támogatja az Azure Blob, Queue és Table storage Azure Storage-fiók. Belsőleg az Azure Functions Azure tárolást használ műveletek, például eseményindítók kezelése és naplózási funkciót végrehajtások. Néhány tárfiókok nem támogatják az üzenetsorok és táblák, például csak a blob storage-fiókok (beleértve a prémium szintű storage) és az általános célú tárfiókok zónaredundáns tárolás replikáció. Ezek a fiókok kiszűri a **Tárfiók** panel egy függvény alkalmazás létrehozásakor.

Tárfióktípusokat kapcsolatos további információkért lásd: [az Azure Storage szolgáltatásainak bemutatása](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>A felhasználási terv működése

A felhasználási terv automatikusan méretezi a CPU és memória-erőforrások, adja hozzá a további példányait a funkciók gazdagép, a funkciók által kiváltott a várakozó események száma alapján. A funkciók állomás minden példánya 1,5 GB memória korlátozódik.

A felhasználás üzemeltetési terv használatakor függvény kódfájlok Azure fájlmegosztásokat a fő tárfiók tárolja. A fő storage-fiók törlésekor a tartalom törlődik, és nem állítható helyre.

> [!NOTE]
> Egy blob eseményindító használatakor a fogyasztás terven lehet legfeljebb 10 perces késleltetést új blobok feldolgozása, ha egy függvény app inaktív állapotba került. A függvény alkalmazás futtatása után blobok feldolgozása azonnal megtörténik. A kezdeti késleltetés elkerülése érdekében fontolja meg az alábbi lehetőségek közül:
> - Használja az App Service-csomag a mindig engedélyezve van.
> - Egy másik mechanizmus használatával indul el, a blob feldolgozására, például egy üzenetsor-üzenetet, amely tartalmazza a blob neve. Egy vonatkozó példáért lásd: [várólista eseményindító blob bemeneti kötése](functions-bindings-storage-blob.md#input-sample).

### <a name="runtime-scaling"></a>Futásidejű skálázás

Az Azure Functions összetevőt használja a *méretezési vezérlő* mértéke a események figyelésére, és határozza meg, hogy ki vagy csökkentheti. A skála vezérlő heurisztikát alkalmaz az egyes eseményindító. Például az Azure Queue storage eseményindító használatakor méretezés a várólista hossza és a legrégebbi üzenetsor korát alapján.

A skálázási egység a függvény alkalmazást. Ha a függvény app horizontálisan, további erőforrásokat az Azure Functions gazdagép több példányát futtatni. Ezzel ellentétben a számítási igény szerinti csökken, a méretezési vezérlő függvény állomás példányok eltávolítja. A példányok száma van végül méretezhető nulla nem működik a funkció alkalmazások futtatásakor.

![Skála vezérlő események figyelése és példányok létrehozása](./media/functions-scale/central-listener.png)

### <a name="billing-model"></a>Számlázási modell

A felhasználási terv részletes leírását lásd a számlázási a [árképzést ismertető oldalra Azure Functions]. Használati függvény alkalmazásszinten összesített értéket, és csak arra az időre, hogy fut-e funkciókódot számát. Számlázási egységei a következők: 
* **Erőforrás-felhasználás (GB-s) GB-másodperc**. Számított, a memóriaméret és a végrehajtási idő összes funkciójának függvény alkalmazásokban. 
* **Végrehajtások**. Minden alkalommal, amikor egy függvény végrehajtása a kötés által kiváltott eseményt, válaszul számítanak.

[árképzést ismertető oldalra Azure Functions]: https://azure.microsoft.com/pricing/details/functions
