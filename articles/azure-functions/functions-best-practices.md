---
title: "az Azure Functions aaaBest eljárásai |} Microsoft Docs"
description: "Az Azure Functions ajánlott eljárásairól és mintáiról megismerése."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "az Azure functions mintázatok, ajánlott eljárás, Funkciók, Eseményfeldolgozási, webhookokkal, a dinamikus számítási, a kiszolgáló nélküli architektúrája"
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/13/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd3d826b75267a740e355b008943a48f71ebd339
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-hello-performance-and-reliability-of-azure-functions"></a>Hello teljesítmény-és az Azure Functions megbízhatóság

Ez a cikk útmutatást tooimprove hello teljesítményéről és megbízhatóságáról, a függvény alkalmazások tartalmazza. 


## <a name="avoid-long-running-functions"></a>Kerülje a hosszú ideig futó funkciók

Nagyméretű, hosszú futású funkciók váratlan időtúllépési hibákat is okozhat. Egy függvény nagy toomany Node.js függőségek miatt válhat. Függőségek importálását is okozhat váratlan időtúllépések eredményező megnövekedett terhelést alkalommal. Függőségek töltődnek be mindkét explicit és implicit módon. A kód által betöltött egyetlen modul töltődhet saját további modulok.  

Amikor csak lehetséges, azonosítóterületen nagy funkciók kisebb függvénynek együtt beállítja, hogy a munka, és térjen vissza a gyors válaszok. Például egy webhook vagy a HTTP-eseményindító függvény előfordulhat, hogy egy visszaigazoló választ belül bizonyos; gyakori, hogy a webhookok toorequire egy azonnali válasz is. A várólista toobe dolgozza fel a várólista funkció be átadhatók hello HTTP eseményindító hasznos. Ez a megközelítés lehetővé teszi toodefer hello tényleges munka, és térjen vissza az azonnali válasz.


## <a name="cross-function-communication"></a>Kereszt-függvény kommunikáció

Több funkciók integrálása, esetén általában a bevált gyakorlat toouse tárüzenetsort a közötti kommunikáció függvény.  hello legfőbb oka tárüzenetsort olcsóbb és sokkal könnyebb tooprovision. 

A tárolási várólistában lévő egyes üzenetek méretének too64 korlátozott KB. Toopass nagyobb üzeneteket funkciók között van szüksége, az Azure Service Bus-üzenetsorba használt toosupport üzenetek méretének mentése too256 KB lehet.

Service Bus-témakörök hasznosak, ha üzenet feldolgozás előtt szűrés van szüksége.

Az Event hubs hasznos toosupport nagy mennyiségű kommunikációs.


## <a name="write-functions-toobe-stateless"></a>Állapot nélküli funkciók toobe írása 

Funkciók kell lennie az állapot nélküli és idempotent Ha lehetséges. Az adatok minden szükséges állapotadatokat társítani. Például egy rendelés feldolgozása valószínűleg a hozzárendelt `state` tag. A következő függvényt egy sorrendet, miközben maga hello függvény állapot nélküli marad, hogy állapotától függően sikerült feldolgozni. 

Az Idempotent funkciók különösen az időzítő eseményindítók használata ajánlott. Például ha rendelkezik, amelyet feltétlenül naponta egyszer kell futtatni, írják, hello nap alatt való futtatás hello ugyanazokat az eredményeket. Ha nincs feladata egy adott napon a következő módokon léphet hello függvény. Is előző toocomplete sikertelen, ha következő futtatás hello kell onnan folytathatja az adatgyűjtést, ahol abbahagyta.


## <a name="write-defensive-functions"></a>Defenzív írást

Tegyük fel, a függvény kivételt bármikor sikerült tapasztal. Tervezze meg a függvények hello képességét toocontinue egy előző sikertelen pont hello következő végrehajtása közben. Vegye figyelembe a következő műveletek hello van szükség:

1. A lekérdezés egy db 10 000 soraihoz.
2. Hozzon létre egy üzenetsor-üzenetet minden e sorok tooprocess további hello sor le.
 
Attól függően, hogy hogyan összetett a rendszer, előfordulhat, hogy: helytelen viselkedik érintett alárendelt szolgáltatásokkal hálózati kimaradások vagy a kvóta korlátozza elérte, stb. A függvény bármikor befolyásolhatja mindegyik. A funkciók toobe az előkészített kell toodesign.

Hogyan reagál a kódját a után 5000 elem beszúrása a várólistára feldolgozásra hiba esetén? Nyomon követheti, hogy befejezte készletben. Ellenkező esetben előfordulhat, hogy be őket újra a következő alkalommal. Ez a munkafolyamat egy súlyos hatása lehet. 

Ha egy elem már megtörtént a feldolgozása, a függvény toobe lehetővé műveletvégzés.

Használhatja a hello Azure Functions platform összetevők már foglalt védelmi intézkedések előnyeit. Lásd például: **elhalt üzenetek kezelésének** hello dokumentációjában [Azure Storage Üzenetsorába elindítja a](functions-bindings-storage-queue.md#trigger).
 

## <a name="dont-mix-test-and-production-code-in-hello-same-function-app"></a>Nem vegyes teszt termelési kód és a hello azonos függvény alkalmazás

A függvény alkalmazások funkciók osztozik az erőforrásokon. Például a memória van osztva. Egy függvény alkalmazást éles környezetben használata, ha nem adja hozzá, teszt kapcsolatos funkciók és erőforrások tooit. Éles kód végrehajtása során váratlan terhelést okozhat.

Ügyeljen az éles függvény alkalmazások betöltése. Memória van átlagosan hello alkalmazásban minden függvény volt.

Ha egy megosztott hivatkozott több .NET-es függvényeket, elhelyezi egy közös megosztott mappába. A következő példa egy utasítás hasonló toohello hivatkozás hello szerelvény: 

    #r "..\Shared\MyAssembly.dll". 

Ellenkező esetben is könnyen tooaccidentally hello közötti másképp viselkednek bináris működik több teszt verzió telepítéséhez.

Ne használja az éles kódban részletes naplózás. Rendelkezik egy negatívan befolyásolta a teljesítményt.



## <a name="use-async-code-but-avoid-blocking-calls"></a>Aszinkron kódot használja, de elkerülni hívások

Aszinkron programozás az ajánlott eljárás. Azonban mindig elkerülése hello hivatkozó `Result` vagy tulajdonság hívása `Wait` metódusa egy `Task` példány. Ez a megközelítés toothread Erőforrásfogyás vezethet.


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a>Következő lépések
További információkért tekintse meg a következő erőforrások hello:

Mivel az Azure Functions használ akkor is tisztában kell lennie az App Service-irányelvek Azure App Service-ben.
* [Minták és gyakorlatok HTTP teljesítményoptimalizálás](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)

