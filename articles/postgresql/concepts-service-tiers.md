---
title: "AAA \"Árképzési szintjeinek PostgreSQL Azure adatbázisban található\""
description: "Árképzési szinteket az Azure PostgreSQL-adatbázishoz"
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: article
ms.date: 05/31/2017
ms.openlocfilehash: f10389dd2ad1ff04e83b02786a407c10140a007b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-options-and-performance-understand-whats-available-in-each-pricing-tier"></a>Azure-adatbázishoz tartozó PostgreSQL beállításai és teljesítménye: az egyes tarifacsomagok elérhető
Egy PostgreSQL-kiszolgálóhoz tartozó Azure-adatbázis létrehozásakor meg határoz három fő választási lehetőség, hogy a kiszolgáló számára lefoglalt tooconfigure hello erőforrások. Ezek a lehetőségek hello teljesítmény és méretezhetőség hello kiszolgáló hatással.
- Tarifacsomag
- Számítási egységek
- Storage (GB)

Minden tarifacsomag tartománnyal rendelkező teljesítmény szintek (számítási egység) toochoose származó, a munkaterhelések követelményeitől függően. Magasabb teljesítményszintet további erőforrásokat a kiszolgáló tervezett toodeliver nagyobb átviteli sebesség eléréséhez adja meg. Módosíthatja a tarifacsomag gyakorlatilag alkalmazásleállás belül hello server teljesítményszint szükséges.

> [!IMPORTANT]
> Hello szolgáltatás pedig nyilvános előzetes verziójában nincs egy garantált szolgáltatási szint szerződés (SLA).

Azure adatbázisban PostgreSQL-kiszolgáló egy vagy több adatbázist is. Részt vesz egy önálló adatbázis / server tooutilize toocreate hello erőforrások, vagy hozzon létre több adatbázisok tooshare hello erőforrásokat. 

## <a name="choose-a-pricing-tier"></a>Válasszon tarifacsomagot
A képen PostgreSQL az Azure-adatbázis két tarifacsomagok kínál: Basic és Standard. Prémium csomagban még nem érhető el, de hamarosan elérhető. 

hello következő táblázat példákat hello tarifacsomagok ajánlott különböző alkalmazások és szolgáltatások olyan alkalmas.

| Tarifacsomag | Kívánt teljesítményprofilok |
| :----------- | :----------------|
| Basic | Kis szolgáltatások, amelyek méretezhető számítási és tárolási IOPS biztosítása nélkül a legalkalmasabb. Ilyen például a fejlesztési vagy tesztelési használt kiszolgálók, vagy csak kevés számítógépet érintő ritkán használt alkalmazások. |
| Standard | Nyissa meg-toooption felhő hello IOPS igénylő alkalmazások biztosítása a nagy átviteli sebességgel. Például a web- vagy analitikai alkalmazások. |
| Prémium | Kis késleltetésű tranzakciók és IO igénylő munkaterheléseknél leginkább megfelelő. Hello legjobb támogatást nyújt a nagyszámú egyidejű felhasználót. Megfelelő toodatabases, amelyek támogatják az alapvető fontosságú alkalmazások.<br />a képen hello prémium tarifacsomag nem érhető el. |

egy árakkal kapcsolatos toodecide réteg, alapján meghatározhatja, hogy a munkaterhelés kell egy IOPS garancia első indításakor. Ha igen, használja a Standard tarifacsomag.

| **Árképzési szint szolgáltatások** | **Basic** | **Standard** |
| :------------------------ | :-------- | :----------- |
| Maximális számítási egység | 100 | 800 | 
| Teljes tároló maximális mérete | 1 TB | 1 TB | 
| Tárolási IOPS garancia | N/A | Igen | 
| Maximális tárolási iops-érték | N/A | 3,000 | 
| Adatbázis biztonsági mentés megőrzési időtartam | 7 nap | 35 nap | 

Hello preview időtartam, alatt árképzési szint hello kiszolgáló létrehozása után nem módosíthatja. A jövőbeli hello azt lehetséges tooupgrade vagy fogja használni a kiszolgálót egy réteg tooanother tarifacsomagot a.

## <a name="understand-hello-price"></a>Hello ár ismertetése
Amikor létrehoz egy új Azure-adatbázis a PostgreSQL belül hello [Azure Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer), kattintson a hello **tarifacsomag** panelen, és a havi költségeket hello megjelennek alapján kiválasztott hello-beállítások. Ha még nem rendelkezik Azure-előfizetéssel, használja a hello Azure árképzési Számológép tooget a becsült ár. Látogasson el a hello [Azure árképzési Számológép](https://azure.microsoft.com/pricing/calculator/) webhelyet, kattintson a **elemek hozzáadása**, bontsa ki a hello **adatbázisok** kategória, és válassza **az Azure-adatbázis PostgreSQL** toocustomize hello-beállítások.

## <a name="choose-a-performance-level-compute-units"></a>Itt választhatja ki a teljesítmény (számítási egység)
Az Azure-adatbázis PostgreSQL-kiszolgáló IP-címek hello meghatározása után szükséges számítási egységek száma hello kiválasztásával áll készen toodetermine hello teljesítményszint szükséges. Jó kiindulási pont 200 vagy 400 számítási egység van szüksége a magasabb szintű felhasználói párhuzamossági webes és analitikai munkaterhelések, és fokozatosan igény szerint módosítsa alkalmazások esetében. 

Számítási egység egy mérték, amely garantáltan toobe elérhető tooa Processzor feldolgozási átviteli Azure-adatbázis egy PostgreSQL-kiszolgáló. A számítási egység mérőszáma kevert CPU és memória-erőforrásokat.  További információkért lásd: [ismertető számítási egység](concepts-compute-unit-and-storage.md)

### <a name="basic-pricing-tier-performance-levels"></a>Alapszintű árképzési szint teljesítményszintet:

| **Teljesítményszint szükséges** | **50** | **100** |
| :-------------------- | :----- | :------ |
| Maximális számítási egység | 50 | 100 |
| Belefoglalt tároló mérete | 50 GB | 50 GB |
| Kiszolgáló tároló maximális\* | 1 TB | 1 TB |

### <a name="standard-pricing-tier-performance-levels"></a>Standard árképzési szint teljesítményszintet:

| **Teljesítményszint szükséges** | **100** | **200** | **400** | **800** |
| :-------------------- | :------ | :------ | :------ | :------ |
| Maximális számítási egység | 100 | 200 | 400 | 800 |
| Belefoglalt és a kiosztott IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS |
| Kiszolgáló tároló maximális\* | 1 TB | 1 TB | 1 TB | 1 TB |
| Kiszolgáló maximális IOPS kiépítve | 3000 IOPS | 3000 IOPS | 3000 IOPS | 3000 IOPS |
| Maximális server kiépített IOPS / GB | Rögzített 3 IOPS / GB | Rögzített 3 IOPS / GB | Rögzített 3 IOPS / GB | Rögzített 3 IOPS / GB |

\*Kiszolgáló tároló maximális toohello maximális kiosztott mérete a kiszolgáló hivatkozik.

## <a name="storage"></a>Storage 
hello tárolási konfigurációt a tárolási kapacitás elérhető tooan Azure adatbázis PostgreSQL-kiszolgáló hello mennyisége határozza meg. hello szolgáltatás által használt hello tárolási hello adatbázisfájlokat, a tranzakciós naplók és a hello PostgreSQL-kiszolgáló naplói tartalmaz. Fontolja meg a szükséges tárterület toohost hello méretét az adatbázisok és hello teljesítménykövetelményeknek (IOPS) lehetőséget választva hello tárolási konfigurációt.

Bizonyos tárolási kapacitás megtalálható legalább az egyes tarifacsomagok feljegyzett hello megelőző a táblázatban "Befoglalt tárméret." További tárolási kapacitás létrehozásakor hello server mentése toohello maximális engedélyezett tárolási 125 GB-os lépésekben lehet hozzáadni. hello további tárolási kapacitás hello számítási egység konfigurációs függetlenül konfigurálhatók. hello ár módosítások a kiválasztott tárolási hello mennyisége alapján.

az egyes hello IOPS konfigurálás tarifacsomag és a kiválasztott hello tárméret toohello vonatkozik. Az alapszintű csomag nem biztosít egy IOPS garantált. Hello Standard tarifacsomagot, belül hello IOPS méretezése arányosan toomaximum tárméret egy rögzített 3:1 arányban. hello szereplő tárolási 125 GB garanciák 375 az iops-érték, egy IO mérete too256 KB fel az kiépítve. További tárhely too1 TB maximális másolatot választhat, tooguarantee 3000 IOPS kiépítve.

Hello metrikák grafikont a hello Azure portál vagy írási Azure CLI parancsok toomeasure hello fogyasztásának tárolási és iops-érték. Kapcsolódó metrikák toomonitor a következők: tárolási kapacitása, tárolási százalékos, használt tárolási és IO százalék.

>[!IMPORTANT]
> Kép nézetben, válassza ki a tárolási hello mennyisége hello kiszolgáló létrehozásakor hello időpontban. A meglévő kiszolgáló hello tároló méretének módosítása még nem támogatott. 

## <a name="scaling-a-server-up-or-down"></a>A kiszolgáló skálázás feljebb vagy lejjebb
Kezdetben választja hello és teljesítményszintet árképzési tartozó PostgreSQL az Azure-adatbázis létrehozásakor. Később, méretezhető számítási egység felfelé vagy lefelé dinamikusan, hello hello tartományán belül azonos hello tarifacsomagra vált. A hello Azure-portálon, csúsztassa be a hello számítási egység hello server árazás réteg panelen vagy parancsfájl az ebben a példában a következő: [figyelő és a skála egy PostgreSQL egykiszolgálós Azure parancssori felület használatával](scripts/sample-scale-server-up-or-down.md)

Skálázás hello számítási egység hello maximális tárolómérete választott függetlenül történik.

Hello háttérben hello teljesítményszint szükséges adatbázis módosítása alkalmazás egy replikát készít hello eredeti adatbázis szinten hello új teljesítményét, és kapcsolatok majd átvált toohello replika. Adatok nem vesztek el a folyamat során. Során hello rövid jelenleg amikor azt vált át toohello replika kapcsolatok toohello adatbázis le van tiltva, ezért néhány útban is lehet tranzakciók visszaállítása. Ennek időtartama eltérő lehet, de átlagosan 4 másodpercnél rövidebb, az esetek 99%-ában pedig kevesebb mint 30 másodperc. Ha nagy számok: hello jelenleg kapcsolatok útban tranzakciók le vannak tiltva, akkor ezt az ablakot is hosszabb lehet.

hello teljes méretezési folyamat időtartamának hello hello mérete és tarifacsomag hello kiszolgáló előtt és után hello módosítása függ. Például egy kiszolgálót, amely a számítási egység jal hello Standard tarifacsomagot, belül kell végeznie néhány percen belül. hello új tulajdonságok hello kiszolgáló nem érvényesek, amíg hello módosítások be nem fejeződik.

## <a name="next-steps"></a>Következő lépések
- A számítási egységek kapcsolatban bővebben lásd: [ismertető számítási egység](concepts-compute-unit-and-storage.md)
- Ismerje meg, hogyan túl[figyelő és a skála egy PostgreSQL egykiszolgálós Azure parancssori felület használatával](scripts/sample-scale-server-up-or-down.md)
