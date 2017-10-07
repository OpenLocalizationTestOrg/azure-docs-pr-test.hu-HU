---
title: "aaaAzure funkciók használati és az App Service-csomagok |} Microsoft Docs"
description: "Ismerje meg, hogyan az Azure Functions méretezi a eseményvezérelt munkaterhelések toomeet hello igényeinek."
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
ms.openlocfilehash: 3826915b93328635d9295efe90ecc421e6c56af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-consumption-and-app-service-plans"></a>Az Azure Functions használati és az App Service tervek 

## <a name="introduction"></a>Bevezetés

Az Azure Functions két különböző módban is futtathatja: fogyasztás terv és az Azure App Service-csomag. a kód fut-e, méretezi kimenő szükséges toohandle terhelés, és majd arányosan csökken, amikor a kód nem fut a hello fogyasztás terv automatikusan számítási teljesítményt foglal le. Igen toopay nincs tétlen virtuális gépekhez, és nem kell előzetesen tooreserve kapacitás. Ez a cikk foglalkozik a hello fogyasztás terv. App Service-csomag hello működésével kapcsolatos részletekért lásd: hello [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Ha nem ismeri az Azure Functions, tekintse meg a hello [Azure Functions áttekintése](functions-overview.md).

Ha létrehoz egy függvény alkalmazást, konfigurálnia kell a üzemeltetési terv függvények hello alkalmazást tartalmaz. Mindkét üzemmódban, hello példányának *Azure Functions állomás* hello funkciók végrehajtja. terv vezérlők hello típusa:

* Hogyan állomás példányok is kiterjeszthető.
* hello erőforrásokhoz elérhető tooeach állomás.

Jelenleg választania kell hello terv típus hello függvény app hello létrehozása során. Ezt követően nem módosítható. 

Az App Service-csomag hello rétegek közötti méretezheti. A hello fogyasztás terv az Azure Functions automatikusan kezeli az összes erőforrás-elosztás.

## <a name="consumption-plan"></a>Használatalapú csomag

Amikor egy fogyasztás tervet használja, hello Azure Functions állomás példányai dinamikusan felvétele, illetve eltávolítása hello bejövő események száma alapján. Ez a csomag automatikusan méretezi, és van szó, a számítási erőforrások csak akkor, ha a függvények futnak. Felhasználás tervezze a függvény legfeljebb 10 perc futtathatja. 

> [!NOTE]
> függvények fogyasztás tervezze hello alapértelmezett időtúllépési érték 5 perc. hello érték nagyobb too10 percig, amíg hello függvény App hello tulajdonságának módosításával lehet `functionTimeout` a [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

A függvény alkalmazások minden funkciók között összesített értéket, és számlázási végrehajtási ideje és a használt memória alapul. További információkért lásd: hello [árképzést ismertető oldalra Azure Functions].

hello fogyasztás terv hello alapértelmezett és hello a következő előnyöket nyújtja:
- Után kell fizetnie, csak ha a függvények futnak.
- Horizontális felskálázás automatikusan, még nagy idején betölteni.

## <a name="app-service-plan"></a>App Service-csomag

Tervezze meg az App Service hello, a függvény alkalmazások futtatása a Basic, Standard és Premium termékváltozat hasonló tooWeb alkalmazások dedikált virtuális gépeken. Dedikált virtuális gépek a lefoglalt tooyour App Service apps, ami azt jelenti, hogy hello funkciók gazdagép mindig fut.

Vegye figyelembe a következő esetekben hello az App Service-csomagot:
- Rendelkezik már meglévő, a túl többi App Service-példány már fut a virtuális gépek.
- A függvény alkalmazások toorun folyamatosan vagy majdnem folyamatosan várt.
- További CPU és memória beállítások, mint mi biztosítja a hello fogyasztás terv van szüksége.
- Hello hello fogyasztás terv engedélyezett maximális végrehajtási idő legfeljebb toorun van szüksége.

A virtuális gépek leválasztja a futásidejű és a memória méretét a költségek. Emiatt nem kell fizetnie több mint hello Virtuálisgép-példány lefoglalandó hello költségét. App Service-csomag hello működésével kapcsolatos részletekért lásd: hello [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Az App Service-csomagot lehet manuálisan horizontálisan további Virtuálisgép-példányok hozzáadásával, vagy engedélyezheti az automatikus skálázási. További információkért lásd: [méretezése példányszám manuális vagy automatikus](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). Is költenie válasszon egy másik App Service-csomagra. További információkért lásd: [vertikális felskálázás az Azure alkalmazásban](../app-service-web/web-sites-scale.md). Ha azt tervezi, toorun JavaScript-funkcióként az App Service-csomagot, válasszon egy tervet, amely kevesebb magok rendelkezik. További információkért lásd: hello [függvények JavaScript hivatkozás](functions-reference-node.md#choose-single-core-app-service-plans).  

<!-- Note: hello portal links toothis section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
###Always On

Ha az App Service-csomagot futtatja, engedélyeznie kell a hello **mindig a** beállítása, hogy a függvény alkalmazás megfelelően fut. Az App Service-csomagot hello functions futtatókörnyezete kerül üresjárati tétlen, néhány perc múlva, csak a HTTP-eseményindítók fogja "felébreszteni" függvényeit. Ez a hasonló toohow WebJobs rendelkeznie kell mindig engedélyezve van. 

Always On érhető csak az App Service-csomag. A fogyasztás terven hello platform aktiválja függvény alkalmazások automatikusan.

## <a name="storage-account-requirements"></a>Tárolási fiókra vonatkozó követelmények

A felhasználási terv vagy az App Service-csomag a függvény az alkalmazás csak a, amely támogatja az Azure Blob, Queue és Table storage Azure Storage-fiók. Belsőleg az Azure Functions Azure tárolást használ műveletek, például eseményindítók kezelése és naplózási funkciót végrehajtások. Néhány tárfiókok nem támogatják az üzenetsorok és táblák, például csak a blob storage-fiókok (beleértve a prémium szintű storage) és az általános célú tárfiókok zónaredundáns tárolás replikáció. Ezek a fiókok hello kiszűri **Tárfiók** panel egy függvény alkalmazás létrehozásakor.

toolearn tárfióktípusokat, kapcsolatos további információkért lásd: [hello Azure Storage szolgáltatás bevezetése](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="how-hello-consumption-plan-works"></a>Hello fogyasztás terv működése

hello fogyasztás terv automatikusan méretezi a CPU és memória-erőforrások hello funkciók gazdagép, a funkciók lépnek működésbe. a események hello száma alapján további példányai hozzáadásával. Minden példány hello funkciók állomás korlátozott too1.5 GB memória.

Használatakor hello üzemeltetési terv fogyasztás, Azure fájlmegosztásokat hello fő tárfiók tárolja függvény kód fájlokat. Hello fő storage-fiók törlésekor a tartalom törlődik, és nem állítható helyre.

> [!NOTE]
> Egy blob eseményindító használatakor a fogyasztás terven lehet tooa 10 perces késleltetést mentése új blobok feldolgozása, ha egy függvény app inaktív állapotba került. Hello függvény alkalmazás futtatása után blobok feldolgozása azonnal megtörténik. a kezdeti tooavoid késleltetés, javasoljuk, hogy hello alábbi beállítások egyikét:
> - Használja az App Service-csomag a mindig engedélyezve van.
> - Használjon egy másik mechanizmust tootrigger hello blob feldolgozására, például egy üzenetsor-üzenetet, amely hello blob nevét tartalmazza. Egy vonatkozó példáért lásd: [várólista eseményindító blob bemeneti kötése](functions-bindings-storage-blob.md#input-sample).

### <a name="runtime-scaling"></a>Futásidejű skálázás

Az Azure Functions hello összetevőt használja *méretezési vezérlő* toomonitor hello események gyakorisága, és döntse el, hogy tooscale csökkentheti vagy megszüntetésekor. hello méretezési vezérlő heurisztikát alkalmaz az egyes eseményindító. Például az Azure Queue storage eseményindító használatakor méretezés hello várólista hossza és a legrégebbi várólista üdvözlőüzenetére hello korát alapján.

hello skála mértékegysége hello függvény alkalmazást. Hello függvény app horizontálisan, ha további erőforrások vannak lefoglalva toorun hello Azure Functions gazdagép több példányát. Ezzel ellentétben a számítási igény szerinti csökken, hello méretezési vezérlő függvény állomás példányok eltávolítja. Ha nem működik a funkció alkalmazások futnak hello példányok száma végül méretezhető toozero le.

![Skála vezérlő események figyelése és példányok létrehozása](./media/functions-scale/central-listener.png)

### <a name="billing-model"></a>Számlázási modell

Hello fogyasztás terv részletes leírását lásd a hello számlázási [árképzést ismertető oldalra Azure Functions]. Használati hello függvény alkalmazási szintű összesített értéket, és csak a funkciókódot futtató hello idő számát. Az alábbiakban hello számlázási egységei: 
* **Erőforrás-felhasználás (GB-s) GB-másodperc**. Számított, a memóriaméret és a végrehajtási idő összes funkciójának függvény alkalmazásokban. 
* **Végrehajtások**. Minden alkalommal, amikor egy függvény végrehajtása a választ, tooan a kötés által elindított esemény számítanak.

[árképzést ismertető oldalra Azure Functions]: https://azure.microsoft.com/pricing/details/functions
