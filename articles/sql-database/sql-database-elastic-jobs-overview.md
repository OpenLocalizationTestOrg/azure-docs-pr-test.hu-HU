---
title: "aaaManaging kiterjesztett felhő adatbázisok |} Microsoft Docs"
description: "Bemutatja a hello rugalmas feladat szolgáltatás"
metakeywords: azure sql database elastic databases
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 6fa47cf2-1162-4534-a206-6e2d95b78580
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: b6d330cd712421b8cba781e835830772e6e5b77e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-scaled-out-cloud-databases"></a>Kiterjesztett felhő adatbázisok kezelése
toomanage kiterjesztett szilánkos adatbázisok hello **rugalmas adatbázis-feladatok** funkció (előzetes verzió) lehetővé teszi, hogy Ön tooreliably Transact-SQL (T-SQL) parancsprogram végrehajtása adatbázisok, beleértve a csoportja között:

* (az alábbiakban ismertetett) adatbázisok egyénileg definiált gyűjteménye
* az összes adatbázis egy [rugalmas készlet](sql-database-elastic-pool.md)
* a shard készletének (használatával létrehozott [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md)). 

## <a name="documentation"></a>Dokumentáció
* [Hello rugalmas feladat összetevőinek telepítése](sql-database-elastic-jobs-service-installation.md). 
* [Ismerkedés a rugalmas feladatok](sql-database-elastic-jobs-getting-started.md).
* [PowerShell-lel feladatok létrehozásához és kezeléséhez](sql-database-elastic-jobs-powershell.md).
* [Létrehozásához és kezeléséhez kimenő Azure SQL-adatbázisok méretezése](sql-database-elastic-jobs-getting-started.md)

**Rugalmas adatbázis-feladatok** jelenleg egy ügyfél által szolgáltatott, Azure Cloud Service, amely lehetővé teszi az ad hoc és ütemezett felügyeleti feladatokat, melyekhez nevezzük hello végrehajtásának **feladatok**. A feladatok könnyen, és megbízhatóan kezelése az Azure SQL-adatbázisok nagy mennyiségű felügyeleti műveletek a Transact-SQL-parancsfájlok tooperform futtatásával. 

![Rugalmas feladat szolgáltatás][1]

## <a name="why-use-jobs"></a>Miért érdemes használni a feladatokat?
**Kezelés**

Egyszerűen tegye sémamódosítások, hitelesítő adatok kezelése, hivatkozás adatfrissítések, teljesítményadat-gyűjtés vagy bérlő (ügyfél) telemetriai adatok gyűjtése.

**Jelentések**

Egy Azure SQL-adatbázisok gyűjteményét egyetlen táblát összesített adatokat.

**Terhelés csökkentése**

Általában kell csatlakoztatni a tooeach adatbázis egymástól függetlenül rendelés toorun Transact-SQL-utasítások, vagy más felügyeleti feladatok. Egy feladat hello feladat tooeach adatbázisban, hello célcsoport-naplózás kezeli. Akkor is meghatározását, karbantartása és megőrizni a Transact-SQL-parancsfájlok toobe hajtotta végre az Azure SQL-adatbázisok csoport között.

**Nyilvántartás**

Feladatok futtatása hello parancsfájl és a napló hello állapotát az egyes adatbázisok végrehajtása. Hiba esetén is kap automatikus próbálkozzon újra.

**Rugalmasság**

Azure SQL-adatbázisok egyéni csoportot, és fut egy feladat ütemezésének meghatározását.

> [!NOTE]
> Hello Azure-portálon csak egy meghatározott funkciókat korlátozott tooSQL Azure rugalmas készletek érhető el. Hello PowerShell API-k tooaccess hello teljes készletét a jelenlegi funkcionalitás használatára.
> 
> 

## <a name="applications"></a>Alkalmazások
* Felügyeleti feladatok, például egy új séma telepítését.
* Hivatkozás adatok-termékinformációk közös frissítése összes adatbázis között. Vagy az ütemezések automatikus frissítések minden hétköznap, óra múlva.
* Építse újra az indexek tooimprove lekérdezések teljesítményét. hello újraépítése lehet konfigurált tooexecute közötti ismétlődő módon, adatbázisok gyűjteménye, mint csúcsidőn.
* Gyűjtése lekérdezés eredményeit az adatbázisok közül egy központi táblába folyamatos jelleggel. Teljesítmény-lekérdezések folyamatosan hajtható végre, és végre tootrigger további feladatok toobe konfigurálva.
* Az adatbázisok elvégzése nagyobb hosszabb futó adatfeldolgozási-lekérdezéseket hajt végre, például hello ügyfél telemetriai gyűjteménye. Eredmények további elemzés céljára egyetlen céltáblába történő összegyűjtése.

## <a name="elastic-database-jobs-end-to-end"></a>Rugalmas adatbázis-feladatok: végpont
1. Telepítse a hello **rugalmas adatbázis-feladatok** összetevőket. További információkért lásd: [rugalmas adatbázis telepítése feladatok](sql-database-elastic-jobs-service-installation.md). Ha hello telepítése meghiúsul, tekintse meg a [hogyan toouninstall](sql-database-elastic-jobs-uninstall.md).
2. Használja a PowerShell API-k tooaccess hello további funkciókat, például az adatbázis egyénileg definiált gyűjtemények, ütemezések hozzáadása és/vagy adatgyűjtési eredmények beállítása létrehozásáról. Hello-portál használata egyszerű telepítés és a feladatok létrehozása/figyelési korlátozott elleni tooexecution egy **rugalmas készlet**. 
3. A feladat-végrehajtás titkosított hitelesítő adatok létrehozása és [hello felhasználói (vagy szerepkör) tooeach adatbázis hozzáadása hello csoport](sql-database-security-overview.md).
4. Hozzon létre egy idempotent T-SQL parancsfájl hello csoport minden adatbázison futtatható. 
5. Hajtsa végre a következő lépések toocreate feladatokat hello Azure-portál használatával: [létrehozása és a rugalmas adatbázis-feladatok kezelése](sql-database-elastic-jobs-create-and-manage.md). 
6. Vagy a PowerShell-parancsfájlok használata: [létrehozása a PowerShell (előzetes verzió) használatával SQL Database rugalmas adatbázis-feladatok kezelése és](sql-database-elastic-jobs-powershell.md).

## <a name="idempotent-scripts"></a>Az Idempotent parancsfájlok
hello parancsfájlok kell [idempotent](https://en.wikipedia.org/wiki/Idempotence). Egyszerű fogalmazva "idempotent" azt jelenti, hogy ha hello parancsfájl sikeres, és újra fut, hello ugyanazt az eredményt következik be. A parancsfájl meghiúsulhat, tootransient hálózati problémák miatt. Ebben az esetben hello feladat automatikusan megpróbálja hello parancsfájl futtatását egy előre definiált ennyiszer desisting előtt. Az idempotent parancsfájl hello azonos vezethet, még akkor is, ha korábban már sikeresen futott kétszer van. 

Egy egyszerű tactic tootest hello meglétét az objektum létrehozása az előtt.  

    IF NOT EXIST (some_object)
    -- Create hello object 
    -- If it exists, drop hello object before recreating it.

Ehhez hasonlóan egy parancsfájlt kell lennie képes tooexecute sikeresen logikailag teszteléséhez a, és azokat a feltételeket elleni talál.

## <a name="failures-and-logs"></a>Hibák és a naplókat
Ha több próbálkozást követően a parancsfájl futása sikertelen, a hello feladat hello hiba jelentkezik, és továbbra is fennáll. A feladat befejeződését követően (azaz a Futtatás elleni hello csoportban lévő összes adatbázis), ellenőrizheti a sikertelen bejelentkezési kísérletek listáját. hello naplók toodebug hibás parancsfájlok részletekkel szolgálnak. 

## <a name="group-types-and-creation"></a>Csoport típusok és létrehozása
A csoportok két fő típusba sorolhatók: 

1. A shard beállítása
2. Egyéni csoportok

Shard set csoportok hozhatók létre hello [skálázáshoz rugalmas adatbáziseszközöket](sql-database-elastic-scale-introduction.md). A szilánkok set csoport létrehozásakor adatbázisok hozzáadásakor vagy eltávolításakor automatikusan hello csoportból. Például egy új shard lesz automatikusan hello csoport Ha hozzáadja toohello shard leképezés. Egy feladat majd is futtathatók hello csoport.

Egyéni csoportok a hello ugyanakkor, szigorúan definiálhatók. Meg kell explicit módon adja hozzá, vagy távolítsa el az adatbázisok egyéni csoportokból. Egy adatbázis hello csoport megszakadásakor hello feladat megpróbál toorun hello parancsfájl hello adatbázison végzett végleges hibát eredményez. Hello Azure-portál jelenleg használatával létrehozott csoportok olyan egyéni csoportok. 

## <a name="components-and-pricing"></a>Összetevők és az árképzés terén
hello következő összetevők működnek együtt az Azure felhőalapú szolgáltatás, amely lehetővé teszi az alkalmi felügyeleti feladatok végrehajtásának toocreate. hello összetevők telepítése és konfigurálása automatikusan történik az előfizetés a telepítés során. Hello szolgáltatások azonosíthatja, mindannyian rendelkezik hello azonos az automatikusan generált név. hello neve egyedi, és hello előtag "edj" követően 21 véletlenszerűen létrehozott karaktereket tartalmaz.

* **Azure Cloud Service**: rugalmas adatbázis-feladatok (előzetes verzió) Azure felhasználói által szolgáltatott felhők kerül hello szolgáltatás tooperform végrehajtásának kért feladatok. Hello portálról hello szolgáltatás telepítése és megtalálható a Microsoft Azure-előfizetése. hello telepített alapértelmezett szolgáltatás fut, a magas rendelkezésre álláshoz két feldolgozói szerepkörök hello legalább. minden egyes feldolgozói szerepkör (ElasticDatabaseJobWorker) hello alapértelmezett mérete a A0 példánya fut. Díjszabási, lásd: [árképzési felhőszolgáltatások](https://azure.microsoft.com/pricing/details/cloud-services/). 
* **Az Azure SQL Database**: hello szolgáltatást használ egy Azure SQL Database hello néven **feladatvezérlő adatbázishoz** toostore hello feladat metaadatai mindegyikét. hello alapértelmezett szolgáltatásréteget egy S0. Díjszabási, lásd: [SQL Database – díjszabás](https://azure.microsoft.com/pricing/details/sql-database/).
* **Az Azure Service Bus**: az Azure Service Bus koordinációs hello munka hello Azure Cloud Service belül van. Lásd: [Service Bus árképzési](https://azure.microsoft.com/pricing/details/service-bus/).
* **Az Azure Storage**: az Azure Storage-fiók használatos diagnosztikai toostore kimeneti hello esemény igénylő problémát további hibakeresési naplózás (lásd: [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](../cloud-services/cloud-services-dotnet-diagnostics.md)). Díjszabási, lásd: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-elastic-database-jobs-work"></a>A rugalmas adatbázis-feladatok működése
1. Egy Azure SQL Database jelöl ki egy **feladatvezérlő adatbázishoz** amely tárolja az összes metaadat-adatainak és állapotának adatokat.
2. hello feladatvezérlő adatbázishoz illetéktelen hello **szolgáltatás feladat** toolaunch és nyomon követése feladatok tooexecute.
3. Két különböző szerepkörök kommunikálnak hello feladatvezérlő adatbázishoz: 
   * Tartományvezérlő: Meghatározza, hogy milyen típusú feladatok szükséges feladatok tooperform hello kért feladat, és az ismételt próbálkozás sikertelen feladatok hozzon létre új feladat feladatok.
   * Feladat a feladat a végrehajtás: Hello feladat feladatokat végzi.

### <a name="job-task-types"></a>Feladattípusok feladat
Több feladat feladatokat hajthat végre feladatok végrehajtásának típusa van:

* ShardMapRefresh: Lekérdezések hello shard térkép toodetermine szilánkok használt összes hello adatbázis
* ScriptSplit: Hello parancsfájl felosztja a kötegek "Ugrás" kimutatásait között
* ExpandJob: Gyermek feladatok létrehozása az egyes adatbázisok a feladatból, amelynek célpontja adatbázisok csoportja
* ScriptExecution: Végrehajtja a megadott hitelesítő adatok használatával egy adott adatbázison parancsfájl
* Dacpac: Vonatkozik DACPAC tooa adott adatbázishoz adott hitelesítő adatok használatával

## <a name="end-to-end-job-execution-work-flow"></a>Végpontok közötti feladat-végrehajtási-munkafolyamat
1. Hello portál vagy a PowerShell API hello használ, a feladatok bekerülnek hello **feladatvezérlő adatbázishoz**. hello feladat a megadott hitelesítő adatok használatával adatbázisok csoportja elleni Transact-SQL parancsfájl végrehajtását kéri.
2. hello vezérlő hello új feladat azonosítja. Munka feladatai jönnek létre, és végre toosplit hello parancsfájl és toorefresh hello csoport adatbázisok. Új feladat utolsó lépésként létrejön, tooexpand hello feladat végrehajtása, és hozzon létre új gyermek feladatok, ahol minden gyermek folyamat megadott tooexecute hello Transact-SQL parancsfájl hello csoportban lévő egyes adatbázison.
3. hello vezérlő gyermek feladatok létrehozott hello azonosítja. Az egyes feladatokhoz hello vezérlő hoz létre, és elindítja egy feladat tooexecute hello feladatparancsfájl adatbázis. 
4. Minden feladat feladat befejezése után hello vezérlő frissíti hello feladatok befejeződött tooa állapotát. 
   Feladat végrehajtása során bármikor hello PowerShell API lehet használt tooview hello feladat végrehajtása aktuális állapotát. Az összes idő a PowerShell API-k jelennek meg az UTC hello által visszaadott. Ha szükséges, a lemondási kérelmet kezdeményezett toostop egy feladat lehet. 

## <a name="next-steps"></a>Következő lépések
[Hello összetevőinek telepítése](sql-database-elastic-jobs-service-installation.md), majd [létrehozása és hozzáadása a napló a tooeach adatbázisban az adatbázisok csoportja hello](sql-database-manage-logins.md). toofurther megérteni a feladat létrehozása és kezelése című [rugalmas adatbázis-feladatok létrehozását és kezelését](sql-database-elastic-jobs-create-and-manage.md). Lásd még: [Ismerkedés a rugalmas feladatok](sql-database-elastic-jobs-getting-started.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->


