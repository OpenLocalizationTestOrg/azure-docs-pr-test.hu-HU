---
title: "aaaConcurrency és munkaterhelés-kezelés az SQL Data Warehouse |} Microsoft Docs"
description: "Ismerje meg a feldolgozási és munkaterhelés-kezelés az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 08/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: 7f7e77aa687760252aed16573b609817ed9111c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Párhuzamossági és munkaterhelés-kezelés az SQL Data Warehouse
toodeliver kiszámítható teljesítményt biztosít, a Microsoft Azure SQL Data Warehouse segít párhuzamossági szintek és erőforrás-hozzárendelések például memória és CPU rangsorolását. Ez a cikk bemutatja a feldolgozási és munkaterhelés-kezelés, elmagyarázza, hogyan mindkét szolgáltatásokat nyújt, és hogy miként szabályozható őket az adatraktár toohello elveit. Az SQL Data Warehouse munkaterhelés-kezelés a tervezett toohelp Többfelhasználós környezetek támogatására. Nem célja a több-bérlős munkaterhelésekhez.

## <a name="concurrency-limits"></a>Feldolgozási korlátok
Az SQL Data Warehouse lehetővé teszi, hogy too1, 024 egyidejű kapcsolatok mentése. Az összes 1024 kapcsolat egyidejű lekérdezések küldje el. Toooptimize átviteli, az SQL Data Warehouse azonban előfordulhat, hogy várólista egyes lekérdezések tooensure, hogy minden egyes lekérdezés megkapja-e a minimális memória biztosítása. A lekérdezés-végrehajtás során Queuing következik be. Üzenetsor-kezelés lekérdezések a amikor növelheti a feldolgozási korlátok elérésekor az SQL Data Warehouse teljes átviteli sebesség biztosításával, hogy aktív lekérdezések beolvasása hozzáférés toocritically szükséges memória-erőforrásokat.  

Feldolgozási korlátok vonatkoznak két fogalom: *párhuzamos lekérdezések* és *egyidejűségi üzembe helyezési ponti*. Az egy lekérdezésben tooexecute, végre kell hajtani hello lekérdezés feldolgozási korlát és a hello párhuzamossági tárolóhely foglalási belül.

* Egyidejű lekérdezések hello végrehajtása azonos hello lekérdezések idő. Az SQL Data Warehouse too32 párhuzamos lekérdezések hello nagyobb DWU mérete legfeljebb támogat.
* Párhuzamossági üzembe helyezési ponti DWU alapján foglal le. Minden 100 DWU 4 párhuzamossági üzembe helyezési ponti biztosít. Például egy DW100 4 párhuzamossági üzembe helyezési ponti foglal le, és DW1000 40 foglal le. Minden egyes lekérdezés használ egy vagy több egyidejű tárhelyek, hello függő [erőforrásosztály](#resource-classes) hello lekérdezés. Hello smallrc erőforrásosztály futó lekérdezések egy feldolgozási tárolóhely felhasználását. Egy magasabb erőforrásosztály futó lekérdezések több egyidejű üzembe helyezési ponti felhasználását.

hello következő táblázatban hello korlátozhatja a párhuzamos lekérdezések és a párhuzamosság tárhelyek hello, különböző DWU méretű.

### <a name="concurrency-limits"></a>Feldolgozási korlátok
| DWU | Maximális párhuzamos lekérdezések | Lefoglalt párhuzamossági tárhelyek |
|:--- |:---:|:---:|
| DW100 |4 |4 |
| DW200 |8 |8 |
| DW300 |12 |12 |
| DW400 |16 |16 |
| DW500 |20 |20 |
| DW600 |24 |24 |
| DW1000 |32 |40 |
| DW1200 |32 |48 |
| DW1500 |32 |60 |
| DW2000 |32 |80 |
| DW3000 |32 |120 |
| DW6000 |32 |240 |

E küszöbértékek valamelyike teljesül, amikor új lekérdezések helyezi várólistára, és végre érkezési sorrendben történő kiküldési alapon.  A lekérdezések befejeződik, és hello a lekérdezések és üzembe helyezési ponti süllyed hello korlátok, várólistára helyezett lekérdezések kiadásakor. 

> [!NOTE]  
> *Válassza ki* lekérdezések végrehajtása kizárólag a dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) vagy a katalógus nézetek nem szabályozza a hello feldolgozási korlátok. Hello rendszer függetlenül hello száma lekérdezések végrehajtása a figyelheti.
> 
> 

## <a name="resource-classes"></a>Erőforrás-osztályok
Erőforrás segítségével szabályozható a memóriafoglalás és CPU-ciklusok tooa lekérdezés megadott osztályokat. Kétféle típusú erőforrás osztályok tooa felhasználói adatbázis-szerepkörök hello formájában rendelhet hozzá. hello kétféle típusú erőforrás-osztályok a következők:
1. Dinamikus erőforrás-osztályok (**smallrc, mediumrc, largerc, xlargerc**) foglaljon le a változó méretű memória, attól függően, hogy hello aktuális DWU. Ez azt jelenti, hogy amikor Ön vertikális felskálázás tooa nagyobb DWU, a lekérdezések automatikusan lekérni memóriáját. 
2. Statikus erőforrás osztályok (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) lefoglalni hello függetlenül attól, hogy ugyanazon memóriamennyiség hello aktuális DWU (feltéve, hogy maga DWU hello elegendő memóriával rendelkezik). Ez azt jelenti, hogy nagyobb dwu-k, a lekérdezéseket is futtathat további erőforrás az osztályok egyidejűleg.

A felhasználók **smallrc** és **staticrc10** kap a kisebb méretű memória, és magasabb szintű párhuzamosság előnyeit. Ezzel szemben, minden felhasználóhoz rendelni túl**xlargerc** vagy **staticrc80** kap nagy mennyiségű memóriát, és ezért a lekérdezések kevesebb egyidejűleg is futtathatók.

Alapértelmezés szerint minden felhasználó tagja hello kis erőforrásosztály, **smallrc**. az eljárás hello `sp_addrolemember` van használt tooincrease hello erőforrásosztály, és `sp_droprolemember` van toodecrease hello erőforrásosztály használt. Például a parancs nőne hello erőforrásosztály a loaduser túl**largerc**:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a>Lekérdezések, amelyek nem fogadják el erőforrás-osztályok

A lekérdezések, amelyek nem tudják igénybe venni egy nagyobb memóriafoglalás néhány típusa van. hello rendszer figyelmen kívül hagyja az erőforrás-osztály terhelése, és mindig futtassa ezeket a lekérdezéseket hello kis erőforrásosztály helyette. Hello kis erőforrásosztály mindig fut, ezeket a lekérdezéseket, ha azok futtathatja, ha a feldolgozási üzembe helyezési ponti nyomás alatt, és nem használja a tárolóhelyek több, mint amennyi szükséges. Lásd: [erőforrás osztály kivételek](#query-exceptions-to-concurrency-limits) további információt.

## <a name="details-on-resource-class-assignment"></a>Az erőforrás osztály-hozzárendelés részletei


Néhány további részleteket a erőforrásosztály:

* *Az ALTER szerepkör* engedély is szükséges toochange hello erőforrásosztály egy felhasználó.
* Bár hozzáadhatja a felhasználói tooone vagy több hello magasabb erőforrás osztályok, akkor a dinamikus erőforrás-osztályok élveznek statikus erőforrás osztályok. Ez azt jelenti, hogy ha a felhasználó hozzá van rendelve a tooboth **mediumrc**(dinamikus) és **staticrc80**(statikus), **mediumrc** hello erőforrás osztály, amely van figyelembe véve.
 * Amikor a felhasználó hozzá van rendelve egy erőforrás osztály egy adott osztály erőforrástípusra (egynél több dinamikus erőforrásosztály vagy több statikus erőforrásosztály) mint toomore, akkor hello legmagasabb erőforrásosztály figyelembe véve. Ez azt jelenti, hogy ha a felhasználó hozzá van rendelve, tooboth mediumrc és largerc, hello magasabb erőforrásosztály (largerc) van figyelembe véve. És ha a felhasználó hozzá van rendelve a tooboth **staticrc20** és **statirc80**, **staticrc80** kell figyelembe venni.
* hello erőforrásosztály hello rendszer rendszergazda felhasználó nem módosítható.

Részletes példákat lásd: [módosul felhasználói erőforrás osztály példa](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>A memóriafoglalás
Vannak előnyei és hátrányai tooincreasing egy felhasználó erőforrásosztály. A lekérdezések egy felhasználó egy erőforrásosztály növelését, ad hozzáférési toomore memóriába, ami azt jelentheti, lekérdezések gyorsabban hajtható végre.  Azonban a magasabb erőforrás osztályok is csökkentheti a hello száma párhuzamos lekérdezések futtatható. Ez a hello kompromisszum nagy mennyiségű memória tooa egyetlen lekérdezés elosztásáról, és így további lekérdezések, memória-foglalásokat, egyidejűleg toorun igénylő között. Ha egy felhasználó kap a lekérdezés számára magas foglalásokat, a más felhasználók nem fogják tudni hozzáférés toothat azonos memória toorun lekérdezést.

a következő táblázat hello hello fenntartott memória mérete tooeach terjesztési DWU- és erőforrás-osztály képezi le.

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a>Memória kiosztásokat eloszlása dinamikus erőforrás osztályok (MB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| DW100 |100 |100 |200 |400 |
| DW200 |100 |200 |400 |800 |
| DW300 |100 |200 |400 |800 |
| DW400 |100 |400 |800 |1,600 |
| DW500 |100 |400 |800 |1,600 |
| DW600 |100 |400 |800 |1,600 |
| DW1000 |100 |800 |1,600 |3,200 |
| DW1200 |100 |800 |1,600 |3,200 |
| DW1500 |100 |800 |1,600 |3,200 |
| DW2000 |100 |1,600 |3,200 |6,400 |
| DW3000 |100 |1,600 |3,200 |6,400 |
| DW6000 |100 |3,200 |6,400 |12,800 |

a következő táblázat hello le van foglalva tooeach terjesztési DWU és statikus erőforrásosztály hello memória képezi le. Vegye figyelembe, hello magasabb erőforrás osztályokkal rendelkezik-e a saját memória toohonor hello globális DWU korlátok csökken.

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a>Memória kiosztásokat eloszlása statikus erőforrás osztályok (MB)
| DWU | staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |100 |200 |400 |400 |400 |400 |400 |400 |
| DW200 |100 |200 |400 |800 |800 |800 |800 |800 |
| DW300 |100 |200 |400 |800 |800 |800 |800 |800 |
| DW400 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW500 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW600 |100 |200 |400 |800 |1,600 |1,600 |1,600 |1,600 |
| DW1000 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW1200 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW1500 |100 |200 |400 |800 |1,600 |3,200 |3,200 |3,200 |
| DW2000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |6,400 |
| DW3000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |6,400 |
| DW6000 |100 |200 |400 |800 |1,600 |3,200 |6,400 |12,800 |

A tábla megelőző hello, láthatja, hogy a lekérdezés egy DW2000 hello a futó **xlargerc** erőforrásosztály kellene hozzáférés too6, 400 MB memória belül hello 60 elosztott adatbázisok mindegyike esetében.  Az SQL Data Warehouse nincsenek 60 terjesztéseket. Ezért toocalculate hello teljes memória lefoglalása egy lekérdezést a megadott erőforrásosztály, hello meghaladja értékeket meg kell szorozni 60.

### <a name="memory-allocations-system-wide-gb"></a>Memória kiosztásokat rendszerszintű (GB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| DW100 |6 |6 |12 |23 |
| DW200 |6 |12 |23 |47 |
| DW300 |6 |12 |23 |47 |
| DW400 |6 |23 |47 |94 |
| DW500 |6 |23 |47 |94 |
| DW600 |6 |23 |47 |94 |
| DW1000 |6 |47 |94 |188 |
| DW1200 |6 |47 |94 |188 |
| DW1500 |6 |47 |94 |188 |
| DW2000 |6 |94 |188 |375 |
| DW3000 |6 |94 |188 |375 |
| DW6000 |6 |188 |375 |750 |

Ebből a táblázatból tartozó rendszerszintű memória-hozzárendelések, láthatja, hogy a lekérdezés egy DW2000 hello xlargerc erőforrás osztályban futó lefoglalt 375 GB memória összesen (6400 MB * 60 terjesztéseket / 1024 tooconvert tooGB) az SQL Data Warehouse hello a teljes keresztül.

hello azonos számítási érvényes toostatic erőforrás osztályok.
 
## <a name="concurrency-slot-consumption"></a>Párhuzamossági tárolóhely-használat  
Az SQL Data Warehouse magasabb erőforrás osztályok futtató további memória tooqueries biztosít. Memória mérete rögzített erőforrás.  Ezért hello lekérdezésenként kevesebb egyidejű lekérdezések végrehajtható hello lefoglalt memória. hello következő tábla ismételten kifejezi összes hello előző fogalmak DWU által elérhető párhuzamossági bővítőhelyek és minden erőforrásosztály által felhasznált hello tárhelyek hello számát jeleníti meg egyetlen nézetben.  

### <a name="allocation-and-consumption-of-concurrency-slots-for-dynamic-resource-classes"></a>Foglalási és felhasználási párhuzamossági tárolóhelyek dinamikus erőforrás-osztályok  
| DWU | Maximális párhuzamos lekérdezések | Lefoglalt párhuzamossági tárhelyek | Üzembe helyezési ponti smallrc által használt | Üzembe helyezési ponti mediumrc által használt | Üzembe helyezési ponti largerc által használt | Üzembe helyezési ponti xlargerc által használt |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |4 |4 |1 |1 |2 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |
| DW400 |16 |16 |1 |4 |8 |16 |
| DW500 |20 |20 |1 |4 |8 |16 |
| DW600 |24 |24 |1 |4 |8 |16 |
| DW1000 |32 |40 |1 |8 |16 |32 |
| DW1200 |32 |48 |1 |8 |16 |32 |
| DW1500 |32 |60 |1 |8 |16 |32 |
| DW2000 |32 |80 |1 |16 |32 |64 |
| DW3000 |32 |120 |1 |16 |32 |64 |
| DW6000 |32 |240 |1 |32 |64 |128 |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a>Foglalási és felhasználási párhuzamossági tárolóhelyek statikus erőforrás osztályok  
| DWU | Maximális párhuzamos lekérdezések | Lefoglalt párhuzamossági tárhelyek |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |4 |4 |1 |2 |4 |4 |4 |4 |4 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |8 |8 |8 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |8 |8 |8 |8 |
| DW400 |16 |16 |1 |2 |4 |8 |16 |16 |16 |16 |
| DW500 | 20| 20| 1| 2| 4| 8| 16| 16| 16| 16|
| DW600 | 24| 24| 1| 2| 4| 8| 16| 16| 16| 16|
| DW1000 | 32| 40| 1| 2| 4| 8| 16| 32| 32| 32|
| DW1200 | 32| 48| 1| 2| 4| 8| 16| 32| 32| 32|
| DW1500 | 32| 60| 1| 2| 4| 8| 16| 32| 32| 32|
| DW2000 | 32| 80| 1| 2| 4| 8| 16| 32| 64| 64|
| DW3000 | 32| 120| 1| 2| 4| 8| 16| 32| 64| 64|
| DW6000 | 32| 240| 1| 2| 4| 8| 16| 32| 64| 128|

Ezek a táblázatok a láthatja, hogy az SQL Data Warehouse fut, DW1000 foglal le, mely legfeljebb 32 egyidejű lekérdezéseket és 40 párhuzamossági tárolóhelyek összesen. Minden felhasználó smallrc fut, ha 32 egyidejű lekérdezések volna kell engedélyezett, mert minden egyes lekérdezés 1 párhuzamossági tárolóhely volna felhasználását. Ha minden felhasználó egy DW1000 mediumrc a futtatást, minden egyes lekérdezés szeretné kiosztani 800 MB eloszlása lekérdezésenként 47 GB memória összesen kiosztható és feldolgozási lenne korlátozott too5 felhasználók (40 párhuzamossági üzembe helyezési ponti / 8 tárolóhely mediumrc felhasználónként).

## <a name="selecting-proper-resource-class"></a>Megfelelő erőforrásosztály kiválasztása  
Bevált gyakorlat az erőforrás-osztályok módosítása helyett toopermanently hozzárendelése felhasználók tooa erőforrásosztály. Például, terhelések tooclustered oszloptárindexű táblákat hoz létre magasabb színvonalú indexeket, amikor több memóriát foglal le. tooensure, hogy terhelés toohigher memória rendelkezik-e, kifejezetten az adatok betöltése a felhasználó létrehozása, és véglegesen rendelje hozzá a felhasználó tooa magasabb erőforrásosztály.
Nincsenek ajánlott eljárások toofollow Itt néhány. Fent említett SQL DW támogatja-e a kétféle típusú erőforrások osztály: erőforrás statikus és dinamikus erőforrás-osztályok.
### <a name="loading-best-practices"></a>Gyakorlati tanácsok betöltése
1.  Ha hello elvárásainak betölti a rendszeres adatmennyiséget, statikus erőforrásosztály érdemes használni. Később további számítási teljesítménnyel rendelkezik, hello adatraktár vertikális felskálázásával tooget tudnak több egyidejű toorun lekérdezi out-of-az-box, hello terhelés felhasználói nem több memóriát igényel.
2.  Ha hello elvárásainak nagyobb terhelés bizonyos esetekben a, dinamikus erőforrásosztály érdemes használni. Később, amikor vertikális felskálázásával tooget további számítási teljesítménnyel rendelkezik, hello terhelés felhasználó kap további memória az a-kész, ezért így gyorsabban hello terhelés tooperform.

hello szükséges memóriát tooprocess terhelések hatékonyan függ betöltött hello tábla és a feldolgozandó adatok mennyiségétől hello hello jellegét. Például adatok közösségi koordináló intézet táblába töltéséhez szükséges néhány memória toolet közösségi koordináló intézet rowgroups optimalizálási elérni. További részletekért lásd: hello Oszlopcentrikus indexek - Adatbetöltési útmutatást.

Ajánlott eljárásként javasoljuk toouse legalább 200MB memória terhelések.

### <a name="querying-best-practices"></a>Gyakorlati tanácsok lekérdezése
Lekérdezések összetettségük függően különböző követelményekkel rendelkezik. Memória lekérdezésenként megnövelni, vagy növekvő hello párhuzamossági mindkét érvényes módon tooaugment hello lekérdezés igényeitől függően a teljes teljesítményt.
1.  Hello elvárásainak rendszeres, összetett lekérdezések esetén (például toogenerate napi és heti jelentések), és nem kell egyidejűségi tootake előnyeit, dinamikus erőforrásosztály, akkor a. Ha hello rendszer további adatokat tooprocess, hello adatraktár vertikális felskálázásával ezért automatikusan nyújt további memória toohello felhasználói hello a lekérdezésnek a futtatása.
2.  Ha hello elvárásainak változó vagy diurnal egyidejűségi minták (például ha hello adatbázis lekérik a webes felhasználói felület széles körben elérhető keresztül), egy statikus erőforrásosztály, akkor a. Később, amikor toodata adatraktár vertikális felskálázásával, hello statikus erőforrásosztály társított hello felhasználó automatikusan kell tudni toorun több egyidejű lekérdezéseket.

Ez számos tényezőtől függ, például hello mennyiségét, lekérdezett adatok hello táblasémákat, és különböző csatlakozást, kiválasztása és a csoport predikátumok hello jellege nem nem triviális, attól függően, hogy a lekérdezés hello kell kiválasztásával megfelelő memóriaengedély. Egy általános tükrözik, rendeljen több memóriát lehetővé teszi lekérdezések toocomplete gyorsabb, de csökkenne teljes feldolgozási hello. Párhuzamossági darabolása nem okoz problémát, ha nem árt túlzott memória lefoglalásakor. toofine-hangolási átviteli különböző változataira jellemző az erőforrás-osztályok tett kísérlet lehet szükség.

Hello alábbi tárolt eljárás toofigure kimenő feldolgozási és memória grant / erőforrásosztály adott SLO és hello legközelebbi legjobb erőforrás osztályra memória intenzív közösségi koordináló intézet műveletekhez közösségi koordináló intézet tábla particionálva adott erőforrás osztályra:

#### <a name="description"></a>Leírás:  
A tárolt eljárás hello célja van:  
1. toohelp felhasználói vizsgálhatja meg erőforrás osztály egy adott SLO / feldolgozási és memória biztosítása. Felhasználói igényeihez tooprovide NULL séma és a tablename ehhez az alábbi hello példában látható módon.  
2. hello legközelebbi legjobb tartozó erőforrásosztály hello memória intensed közösségi koordináló intézet műveleteket (terhelésétől, a másolási tábla rebuild index stb.) a megadott erőforrásosztály nem particionált közösségi koordináló intézet táblázat elháríthassa toohelp felhasználó. hello tárolt eljárás tábla séma toofind kimenő hello szükséges memória biztosítása a használja.

#### <a name="dependencies--restrictions"></a>Függőségek és korlátozások:
- A tárolt eljárás nincs particionálva közösségi koordináló intézet tábla tervezett toocalculate memóriakövetelményét.    
- A tárolt eljárás nem memóriakövetelményét CTAS/INSERT-VÁLASZTANI VÁLASSZA részét hello figyelembe veszi, és azt feltételezi, hogy toobe egy egyszerű jelöljön ki.
- A tárolt eljárás egy ideiglenes táblát használ, így ez használható hello munkamenetben hol jött létre a tárolt eljáráshoz.    
- A tárolt eljárás hello aktuális offerings (pl. hardverkonfiguráció, DMS config) függ, és ha bármelyik, amely módosítja majd a tárolt eljárás nem megfelelően fog működni.  
- A tárolt eljárás attól függ, meglévő felajánlott feldolgozási korlátja, és ha megváltozik, majd a tárolt eljárás nem megfelelően fog működni.  
- A tárolt eljárás attól függ, meglévő erőforrás osztály ajánlatokat, és ha, amely módosítja majd ez tárolása megfelelő proc wuold nem működik.  

>  [!NOTE]  
>  Ha nem kap kimeneti megadva, előfordulhat, hogy két esetben paraméterekkel tárolt eljárás végrehajtása után. <br />1. Vagy DW paraméter érvénytelen SLO értéket tartalmazza: <br />2. VAGY nem megfelelő erőforrásosztály közösségi koordináló intézet művelet Ha megadva tábla neve. <br />Például: DW100, legmagasabb rendelkezésre memóriaengedély 400MB és széles táblaséma esetén elegendő toocross hello követelmény 400 MB.
      
#### <a name="usage-example"></a>Példa:
Szintaxis:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. @DWU:Adjon meg egy NULL paraméter tooextract aktuális DWU az Adatraktár-adatbázisban hello hello, vagy adjon meg, az "DW100" hello formájában DWU támogatott
2. @SCHEMA_NAME:Adjon meg egy séma hello tábla neve
3. @TABLE_NAME:Adjon meg egy tábla nevét hello érdeklő

A tárolt eljárás végrehajtása példák:  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

A fenti példák hello használt Table1 sikerült létrehozni, az alábbi:  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-hello-stored-procedure-definition"></a>Íme hello tárolt eljárás definíciója:
```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(   @DWU VARCHAR(7),
      @SCHEMA_NAME VARCHAR(128),
       @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for hello current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need toosupply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) toohold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping tootheir corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="query-importance"></a>Lekérdezés fontossága
Az SQL Data Warehouse erőforrás osztályok munkaterhelés-csoport használatával valósít meg. Nincsenek összesen nyolc hello erőforrás osztályok hello viselkedését szabályozó hello között különböző DWU méretű munkaterhelés-csoport. A DWU az SQL Data Warehouse csak közül négyet használ hello nyolc munkaterhelés-csoport. Ez teljesen logikus, mert egyes tevékenységprofil-csoport hozzá van rendelve a négy erőforrás osztályok tooone: smallrc, mediumrc, largerc, vagy xlargerc. hello hello munkaterhelés csoportok ismertetése fontosságát az, hogy a munkaterhelés csoportok beállítása toohigher *fontossági*. Fontos használt CPU ütemezés. A nagyon fontos futtatása lekérdezések háromszor több, mint a közepes fontos CPU-ciklusok fogja kapni. Ezért a feldolgozási tárolóhely hozzárendelések is CPU prioritásának meghatározása. Lekérdezés 16 vagy több üzembe helyezési ponti használ fel, ha fut, nagyon fontos.

hello következő táblázatban minden tevékenységprofil-csoport leképezéseit hello fontosságát.

### <a name="workload-group-mappings-tooconcurrency-slots-and-importance"></a>Munkaterhelési csoport hozzárendelések tooconcurrency tárhelyek és fontossága
| Munkaterhelés-csoport | Párhuzamossági tárolóhely leképezése | MB / terjesztési | Fontos leképezése |
|:--- |:---:|:---:|:--- |
| SloDWGroupC00 |1 |100 |Közepes |
| SloDWGroupC01 |2 |200 |Közepes |
| SloDWGroupC02 |4 |400 |Közepes |
| SloDWGroupC03 |8 |800 |Közepes |
| SloDWGroupC04 |16 |1,600 |Magas |
| SloDWGroupC05 |32 |3,200 |Magas |
| SloDWGroupC06 |64 |6,400 |Magas |
| SloDWGroupC07 |128 |12,800 |Magas |

A hello **foglalási és felhasználási párhuzamossági tárolóhelyek** diagram, ellenőrizheti, hogy egy DW500 használ 1, 4, 8, vagy 16 párhuzamossági üzembe helyezési ponti smallrc, mediumrc, largerc és xlargerc, illetve. Megtekintheti ezeket az értékeket a diagram toofind hello fontos az egyes erőforrás megelőző hello.

### <a name="dw500-mapping-of-resource-classes-tooimportance"></a>Erőforrás-osztályok tooimportance DW500 leképezése
| Erőforrásosztály | A tevékenységprofil-csoport | Párhuzamossági tárhelyek használt | MB / terjesztési | Fontos |
|:--- |:--- |:---:|:---:|:--- |
| smallrc |SloDWGroupC00 |1 |100 |Közepes |
| mediumrc |SloDWGroupC02 |4 |400 |Közepes |
| largerc |SloDWGroupC03 |8 |800 |Közepes |
| xlargerc |SloDWGroupC04 |16 |1,600 |Magas |
| staticrc10 |SloDWGroupC00 |1 |100 |Közepes |
| staticrc20 |SloDWGroupC01 |2 |200 |Közepes |
| staticrc30 |SloDWGroupC02 |4 |400 |Közepes |
| staticrc40 |SloDWGroupC03 |8 |800 |Közepes |
| staticrc50 |SloDWGroupC03 |16 |1,600 |Magas |
| staticrc60 |SloDWGroupC03 |16 |1,600 |Magas |
| staticrc70 |SloDWGroupC03 |16 |1,600 |Magas |
| staticrc80 |SloDWGroupC03 |16 |1,600 |Magas |

Hello hibaelhárításához a következő DMV-lekérdezés toolook hello különbségeit memória erőforrás-elosztás részletesen hello szempontjából erőforrás-vezérlő hello vagy tooanalyze aktív és előzménynaplók használatát hello munkaterhelés-csoport a következő is használhatja.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                        AS node_type
    ,pn.pdw_node_id                    AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024                AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                    AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                        AS group_max_dop
    ,wg.effective_max_dop                AS group_effective_max_dop
    ,wg.total_request_count                AS group_total_request_count
    ,wg.total_queued_request_count            AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT    pool_name
,        pool_max_mem_MB
,        group_name
,        group_importance
,        (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,        node_name
,        node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,    group_request_max_memory_grant_pcnt
,    group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Feldolgozási korlátok tiszteletben lekérdezések
A legtöbb lekérdezések erőforrás osztályok vonatkozik. Ezeket a lekérdezéseket hello párhuzamos lekérdezés és a párhuzamosság tárolóhely küszöbértékek kell férnie. A felhasználó nem választhat tooexclude lekérdezés hello párhuzamossági tárolóhely modell.

tooreiterate, hello következő utasításokat tiszteletben erőforrás osztályok:

* INSERT-KIVÁLASZTÁSA
* FRISSÍTÉS
* TÖRLÉSE
* Válassza ki a (felhasználói táblák lekérdezésekor)
* ALTER INDEX REBUILD
* ALTER INDEX ÁTSZERVEZ
* ALTER TABLE REBUILD
* INDEX LÉTREHOZÁSA
* HOZZON LÉTRE FÜRTÖZÖTT OSZLOPCENTRIKUS INDEXET
* TABLE AS SELECT (CTAS) LÉTREHOZÁSA
* Az adatok betöltése
* Az adatátviteli műveletek hello adatok adatátviteli szolgáltatás (DMS) által

## <a name="query-exceptions-tooconcurrency-limits"></a>Lekérdezés kivételek tooconcurrency korlátok
Egyes lekérdezések nem fogadják el hello erőforrás osztály toowhich hello felhasználó hozzá van rendelve. A kivételek toohello feldolgozási korlátok válnak, amikor egy adott parancshoz szükséges hello memória-erőforrások terhelése alacsony, gyakran mivel hello parancs a metaadat-művelet. hello ezeket a kivételeket célja tooavoid nagyobb memória helylefoglalását lekérdezések, amelyek soha nem kell őket. Ezekben az esetekben a tényleges erőforrásosztály hello függetlenül mindig használatra a kis erőforrásosztály (smallrc) hello alapértelmezett hozzárendelt toohello felhasználó. Például `CREATE LOGIN` mindig smallrc fog futni. hello szükséges erőforrások toofulfill ezt a műveletet olyan nagyon alacsony, így nem tesz logika tooinclude hello lekérdezés hello párhuzamossági tárolóhely modellben.  Ezeket a lekérdezéseket is nincs korlátozva hello 32 felhasználói feldolgozási korlát, korlátlan számú ezeket a lekérdezéseket futtathat toohello munkamenet legfeljebb 1024 munkamenetek fel.

a következő utasítások hello nem fogadják el az erőforrás osztályok:

* DROP TABLE vagy létrehozása
* AZ ALTER TABLE... KAPCSOLÓ, a megosztott vagy a partíció EGYESÍTÉSE
* AZ ALTER INDEX LETILTÁSA
* A DROP INDEX
* LÉTREHOZÁSI, frissítési vagy a DROP STATISTICS
* A TRUNCATE TABLE
* AZ ALTER ENGEDÉLYEZÉSI
* BEJELENTKEZÉS LÉTREHOZÁSA
* CREATE, ALTER és a DROP USER
* CREATE, ALTER vagy DROP ELJÁRÁST
* Vagy DROP NÉZET létrehozása
* ÉRTÉKEK BESZÚRÁSA
* Válassza ki a rendszer nézetek és dinamikus felügyeleti nézetek
* MAGYARÁZÓ
* DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <a name="changing-user-resource-class-example"></a>A felhasználói erőforrás osztály példa módosítása
1. **Hozzon létre bejelentkezési:** nyissa meg a kapcsolat tooyour **fő** adatbázis-hello az SQL Data Warehouse-adatbázist futtató SQL-kiszolgálón, és hajtsa végre a következő parancsok hello.
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > Egy jó ötlet toocreate egy felhasználó fő adatbázis hello Azure SQL Data Warehouse felhasználók. A felhasználó létrehozása a fő lehetővé teszi, hogy egy felhasználó toologin, például az SSMS használatával adatbázis nevének megadása nélkül.  Lehetővé teszi őket toouse hello object explorer tooview összes adatbázist egy SQL-kiszolgálón.  További létrehozásával és felhasználók kezelésével kapcsolatos további információkért lásd: [az SQL Data Warehouse adatbázis védelme][Secure a database in SQL Data Warehouse].
   > 
   > 
2. **Az SQL Data Warehouse-felhasználó létrehozása:** nyissa meg a kapcsolat toohello **SQL Data Warehouse** adatbázis, és hajtsa végre a következő parancs hello.
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. **Engedélyek:** hello a következő példa engedélyezi `CONTROL` a hello **SQL Data Warehouse** adatbázis. `CONTROL`a hello adatbázis szintje hello egyenértékű az SQL Server db_owner.
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW toonewperson;
    ```
4. **Erőforrásosztály növelése:** használata hello következő lekérdezés tooadd felhasználói tooa nagyobb munkaterhelés felügyeleti szerepkört.
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. **Erőforrásosztály csökkentése:** használata hello következő lekérdezés tooremove a felhasználó az alkalmazások és szolgáltatások felügyeleti szerepkör.
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > Már nem lehetséges tooremove smallrc egy felhasználót.
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a>Várólistára helyezett lekérdezés felderítését és egyéb dinamikus felügyeleti nézetek
Használhatja a hello `sys.dm_pdw_exec_requests` DMV tooidentify lekérdezések, amelyek a feldolgozási sorban várnak. Lekérdezi a feldolgozási tárhely állapottal fog rendelkezni várakozással **felfüggesztve**.

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Alkalmazások és szolgáltatások felügyeleti szerepköre megtekinthetők az `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

a következő lekérdezés hello jeleníti meg, milyen szerepkört minden felhasználóhoz hozzá van rendelve.

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

Az SQL Data Warehouse rendelkezik hello következő várjon típusok:

* **LocalQueriesConcurrencyResourceType**: hello párhuzamossági tárolóhely keretrendszer kívül elhelyezkedik lekérdezések. DMV lekérdezések és a rendszer funkciókkal, mint például `SELECT @@VERSION` példák a helyi lekérdezések.
* **UserConcurrencyResourceType**: hello párhuzamossági tárolóhely keretrendszer belül elhelyezkedik lekérdezések. Végfelhasználói táblák lekérdezéseket képviselő példák, amelyek szeretné használni az erőforrástípus.
* **DmsConcurrencyResourceType**: megvárja-e az adatátviteli műveletek eredő.
* **BackupConcurrencyResourceType**: A várakozás azt jelzi, hogy egy adatbázis biztonsági mentése van folyamatban. az erőforrástípus hello maximális értéke: 1. Ha több biztonsági mentés: hello kérték ugyanannyi időt vesz igénybe, mások várólistájára hello.

Hello `sys.dm_pdw_waits` DMV lehet használt toosee erőforrások kérést vár.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,    SESSION_ID()            AS Current_session
,    s.[status]            AS Session_status
,    s.[login_name]
,    s.[query_count]
,    s.[client_id]
,    s.[sql_spid]
,    r.[command]            AS Request_command
,    r.[label]
,    r.[status]            AS Request_status
,    r.[submit_time]
,    r.[start_time]
,    r.[end_compile_time]
,    r.[end_time]
,    DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,    DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,    DATEDIFF(ms,r.[end_compile_time],r.[end_time])        AS Request_execution_time_ms
,    r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID();
```

Hello `sys.dm_pdw_resource_waits` DMV csak hello erőforrás vár egy adott lekérdezésre által felhasznált jeleníti meg. Erőforrás várakozási idő csak a megadott erőforrások toobe Várakozás hello idő méri, megakadályozását toosignal várnia kell, amely hello ideje szerint az SQL kiszolgáló tooschedule hello lekérdezés alakzatot hello CPU alapjául szolgáló hello szükséges.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID();
```

Hello `sys.dm_pdw_wait_stats` történelmi trendelemzés vár a DMV is használható.

```sql
SELECT    w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Következő lépések
Adatbázis-felhasználók és biztonsági kezelésével kapcsolatos további információkért lásd: [az SQL Data Warehouse adatbázis védelme][Secure a database in SQL Data Warehouse]. További információ a hogyan nagyobb erőforrás osztályok javíthatja a fürtözött oszlopcentrikus index minőségének, lásd: [indexek tooimprove szegmens minőségi újraépítése].

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[indexek tooimprove szegmens minőségi újraépítése]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
