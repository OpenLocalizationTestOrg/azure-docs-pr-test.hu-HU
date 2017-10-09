---
title: "az Azure SQL Database kimenő aaaScaling |} Microsoft Docs"
description: "A szolgáltatott szoftverként (SaaS) fejlesztők szoftverként egyszerűen létrehozhat rugalmas, méretezhető adatbázisok hello felhőalapú ezekkel az eszközökkel"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: d15a2e3f-5adf-41f0-95fa-4b945448e184
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: 82a561e07389d8619727a540fa9424248c087eda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-out-with-azure-sql-database"></a>Scaling out with Azure SQL Database (Horizontális felskálázás az Azure SQL Database segítségével)
Azure SQL Database-adatbázisok hello könnyen lehet horizontálisan **Elastic Database** eszközök. Az eszközök és a szolgáltatások lehetővé teszik, hogy szinte korlátlanul hello használata adatbázis-erőforrások **Azure SQL Database** toocreate megoldások tranzakciós munkaterhelések, és különösen a szoftver egy szolgáltatott szoftverként (SaaS) alkalmazásként. A rugalmas adatbázis-szolgáltatások hello következő állnak:

* [A rugalmas adatbázis ügyféloldali kódtár](sql-database-elastic-database-client-library.md): hello ügyféloldali kódtár olyan szolgáltatás, amely lehetővé teszi a toocreate és szilánkos adatbázisok karbantartása.  Lásd: [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md).
* [A rugalmas adatbázis vegyes egyesítéses eszköz](sql-database-elastic-scale-overview-split-and-merge.md): szilánkos adatbázisok között mozgatja az adatokat. Ez akkor hasznos, az adatok áthelyezése egy több-bérlős adatbázis tooa single-bérlő adatbázis (vagy fordítva). Lásd: [rugalmas adatbázis vegyes egyesítéses eszköz oktatóanyag](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Rugalmas adatbázis-feladatok](sql-database-elastic-jobs-overview.md) (előzetes verzió): használata Azure SQL-adatbázisok toomanage nagyszámú feladatokat. Egyszerűbben végezhetők el a felügyeleti műveletek, például a változás történt, a hitelesítő adatok kezelése, a hivatkozás frissítéseket, a teljesítményadat-gyűjtés vagy a bérlő (ügyfél) telemetriai adatgyűjtési feladatok segítségével.
* [A rugalmas adatbázis-lekérdezés](sql-database-elastic-query-overview.md) (előzetes verzió): lehetővé teszi több adatbázist is toorun Transact-SQL-lekérdezést. Ez lehetővé teszi, hogy a kapcsolat tooreporting eszközök, például az Excel, a Power BI, a Tableau, stb.
* [Rugalmas tranzakciók](sql-database-elastic-transactions-overview.md): Ez a szolgáltatás lehetővé teszi az Azure SQL Database adatbázisok kiterjedő toorun tranzakciók. A rugalmas adatbázis-tranzakció érhetők el a .NET-alkalmazásokban ADO .NET használatával, és hello megszokott programozási környezetet hello segítségével integrálja [System.Transaction osztályok](https://msdn.microsoft.com/library/system.transactions.aspx).

hello az alábbi ábra mutatja egy architektúra, amely tartalmazza az hello **rugalmas adatbázis-szolgáltatások** adatbázisok kapcsolat tooa gyűjteményében.

A kép hello adatbázis színek sémák határoz meg. Adatbázisok hello a megosztáshoz szín hello azonos sémából.

1. Egy **Azure SQL-adatbázisok** Azure-ban horizontális architektúrát működtetnek.
2. Hello **Elastic Database ügyféloldali kódtárának** használt toomanage egy shard van beállítva.
3. Hello adatbázisok részhalmazának engedélyeznie kell egy **rugalmas készlet**. (Lásd: [Mi az, hogy a tárolókészlet?](sql-database-elastic-pool.md)).
4. Egy **rugalmas feladat** futtatásra ütemezve vagy alkalmi T-SQL szkriptek összes adatbázis használatát.
5. Hello **vegyes egyesítéses eszköz** egy shard tooanother használt toomove adatokat jelzi.
6. Hello **rugalmas adatbázis-lekérdezés** lehetővé teszi a toowrite hello shard készlet összes adatbázis kiterjedő lekérdezés.
7. **Rugalmas tranzakciók** teszi lehetővé, amelyek több adatbázisok több toorun tranzakciók. 

![Rugalmasadatbázis-eszközök][1]

## <a name="why-use-hello-tools"></a>Hello eszközök miért érdemes használni?
Eléréséhez a rugalmasság és a skála a felhőalapú alkalmazások volt a virtuális gépek és a blob storage – egyszerű egyszerűen adja hozzá vagy egységek kivonandó időnél, vagy növelje a power. Azonban ez az állapot-nyilvántartó adatok feldolgozása a relációs adatbázisok kihívást maradt. Kihívást kiderült, a következő használati helyzetekben:

* Növekvő és zsugorítását kapacitás hello relációs adatbázis része, a munkaterhelés számára.
* Adatok – például egy különösen elfoglalt end-ügyfél (bérlő) egy adott részének érintő esetleg felmerülő elérési pontokhoz történő kezelése.

Hagyományosan helyzetekben, például ezek eddig említett által az adatbázis-alkalmazásban nagyobb méretű kiszolgálók toosupport hello. Azonban ez a beállítás csak korlátozott hello felhő, ahol minden feldolgozás történik, az előre definiált hagyományos hardvereken. Ehelyett terjesztésére adatokat, és több azonos strukturált adatbázis közötti feldolgozása (a kibővített minta "horizontális" néven ismert) biztosít egy alternatív tootraditional méretezett megközelítések költség-és a rugalmasság.

## <a name="horizontal-and-vertical-scaling"></a>Vízszintes és függőleges skálázás
hello az alábbi ábra hello vízszintes és függőleges dimenziók a méretezés, amely módon hello alapszintű hello rugalmas adatbázisok is méretezhető.

![Vízszintes és függőleges scaleout beállítása][2]

Horizontális skálázás hivatkozik tooadding vagy eltávolításakor adatbázisok rendelés tooadjust kapacitás vagy általános teljesítményét. Ez is feltüntettük "skálázás". Horizontális, amelyben adatok particionálása keresztül azonos strukturált adatbázisok gyűjteménye egy közös módon tooimplement horizontális skálázást.  

Tooincreasing vagy a teljesítményszintjének hello egyedi adatbázis csökkentése függőleges skálázás hivatkozik – ezt is nevezik "vertikális felskálázásával."

A legtöbb felhőméretű adatbázis-alkalmazások ezen két stratégiák kombinációja fogja használni. A szoftver a szolgáltatásalkalmazás használhatja például horizontális skálázási tooprovision új end-ügyfelek és a méretezés tooallow egyes end-ügyfelek adatbázis toogrow vagy Zsugorítás erőforrások igényei szerint hello munkaterhelés függőleges.

* Hello horizontális skálázás áll [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md).
* Függőleges skálázás teszi lehetővé az Azure PowerShell parancsmagok toochange hello szolgáltatási rétegben, vagy úgy, hogy adatbázisok rugalmas készlethez.

## <a name="sharding"></a>Horizontális
*Horizontális* van egy technika toodistribute nagy mennyiségű adat azonos strukturált független adatbázisok méretét. Általában különösen szoftver létrehozása a végfelhasználók vagy a vállalatok számára a szolgáltatás (SAAS) ajánlatok felhő fejlesztőkkel. Ezen end ügyfelek általában hivatkozott tooas "bérlő". Horizontális szükséges bármely számos oka lehet:  

* hello teljes adatok mérete túl nagy toofit belül egy önálló adatbázis hello korlátai
* hello tranzakció átviteli hello teljes munkaterhelés meghaladja egy önálló adatbázis hello képességei
* Bérlők szükség lehet egymástól, fizikai elkülönítése, így külön adatbázisok van szükség az egyes bérlők számára
* Adatbázis különböző részeit különböző földrajzi területeken tooreside szükséges megfelelőségi-, teljesítmény- vagy geopolitikai okok miatt.

Más esetekben, például az adatfeldolgozást az elosztott eszközökről származó adatok horizontális lehet használt toofill egy adatbázisok készleteit tartalmazó ideiglenesen vannak rendezve. Például egy másik adatbázist is dedikált tooeach nap vagy hét. Ebben az esetben hello horizontális kulcs lehet egy egész számot jelölő hello dátum (hello szilánkos tábla összes sora szerepel), és hello alkalmazás toohello hello tartományra kiterjedő adatbázisok részhalmazát kell továbbítani, lekérdezések dátumtartomány adatainak lekérése során kérdés.

Horizontális működik optimálisan, ha az alkalmazás minden tranzakció lehet korlátozott tooa egyetlen horizontális kulcs érték. Amely biztosítja, hogy az összes tranzakció helyi tooa adott adatbázis.

## <a name="multi-tenant-and-single-tenant"></a>Több-bérlős és a bérlői egyetlen
Egyes alkalmazások használata hello legegyszerűbb módszere az önálló adatbázist hoz létre az egyes bérlők számára. Ez a hello **egybérlős horizontális mintát** biztosítja az elkülönítési, a biztonsági mentés/visszaállítás lehetősége és a méretezés hello részletességű hello bérlő erőforrás. Egybérlős horizontális az egyes adatbázisok vagy társítva egy adott bérlő azonosító értékét (felhasználói kulcs értékét), de kulcs nem minden esetben kell hello adatokat mozgatná szerepel. Azt az hello alkalmazás felelősségi tooroute minden egyes kérelem-toohello megfelelő adatbázis - és hello ügyféloldali kódtár egyszerűbbé teheti ezt.

![Több-bérlős és egybérlős][4]

Más forgatókönyvek csomag több bérlő együtt az adatbázisok, nem pedig az önálló adatbázisok elszigetelése őket. Ez az egy tipikus **több-bérlős horizontális mintát** - és vezethetik hello tényt, hogy egy alkalmazás kezeli-e nagy mennyiségű nagyon kis bérlők. A több-bérlős horizontális hello hello adatbázistáblák sorai minden tervezett toocarry egy kulcs, amely azonosítja a hello azonosító vagy horizontális bérlőkulcs. Hello alkalmazásszinten jelentkezett újra, a bérlő kérelem toohello megfelelő adatbázis útválasztási felelős, és ez hello elastic database ügyféloldali kódtár támogatja. Emellett a sorszintű biztonság lehet használt toofilter mely sorai, minden bérlői hozzáférhessenek - további információkért lásd: [több-bérlős alkalmazásokhoz a rugalmas adatbáziseszközöket és sorszintű biztonság](sql-database-elastic-tools-multi-tenant-row-level-security.md). Hello több-bérlős horizontális mintával szükséges verziójának terjesztése adatbázisok között, és ez megkönnyíthető hello rugalmas adatbázis vegyes egyesítéses eszköz. További információ a rugalmas készleteket használó SaaS-alkalmazások szerkezeti kialakításainak toolearn lásd [Tervminták több-bérlős SaaS-alkalmazásokhoz az Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-toosingle-tenancy-databases"></a>Több toosingle-bérlőhöz adatbázisból származó adatok áthelyezése
Egy SaaS-alkalmazás létrehozása, esetén jellemző toooffer lehetséges ügyfelek próbaverziójának hello szoftver. Ebben az esetben egy több-bérlős adatbázis hello adatok költséghatékony toouse. Azonban az ügyfél egy potenciális válásakor single-bérlő adatbázis jobb, mivel jobb teljesítményt nyújt. Ha hello ügyfél hozna létre adatok hello próbaidőszak során, akkor hello [vegyes egyesítéses eszköz](sql-database-elastic-scale-overview-split-and-merge.md) hello több-bérlős toohello új single-bérlő adatbázis toomove hello adatait.

## <a name="next-steps"></a>Következő lépések
Egy mintaalkalmazás, amely bemutatja a hello ügyféloldali kódtár, lásd: [Ismerkedés a rugalmas Datababase eszközök](sql-database-elastic-scale-get-started.md).

tooconvert meglévő adatbázisok toouse hello eszközök című [áttelepítése a meglévő adatbázisok tooscale kibővített](sql-database-elastic-convert-to-use-elastic-tools.md).

toosee hello mintaadatokról hello rugalmas készlet, lásd: [rugalmas készletek ára és teljesítménye szempontok](sql-database-elastic-pool.md), vagy hozzon létre egy új készletet a [rugalmas készletek](sql-database-elastic-pool-manage-portal.md).  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

