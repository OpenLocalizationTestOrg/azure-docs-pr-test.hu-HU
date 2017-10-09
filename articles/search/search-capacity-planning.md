---
title: "az Azure Search tervezési aaaCapacity |} Microsoft Docs"
description: "Állítsa be az Azure Search, ahol az egyes erőforrások árképzéséről számlázható keresési egység a partícióazonosító és másodpéldány számítógép-erőforrásokat."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 1dc16afe-56f9-439d-8874-1733ae1a2b74
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 02/08/2017
ms.author: heidist
ms.openlocfilehash: 4bbbb929a36b932ea7af12e494ca095d98b9005e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-resource-levels-for-query-and-indexing-workloads-in-azure-search"></a>A lekérdezés és a munkaterhelések az Azure Search indexelő erőforrás szintjeinek
Miután [válasszon egy tarifacsomagot](search-sku-tier.md) és [egy keresési szolgáltatás kiépítése](search-create-service-portal.md), hello következő lépésre toooptionally növekedése hello száma replikák és a szolgáltatás által használt partíciók. Minden egyes kínál, számlázási egységek rögzített száma. Ez a cikk azt ismerteti, hogyan tooallocate ezen egységek tooachieve egy optimális konfigurációt, hogy a lekérdezés-végrehajtás indexelő és tárolási követelményeinek.

Erőforrás beállítási lehetőségek érhetők el, ha beállít egy hello szolgáltatást [alapszintű rétegben](http://aka.ms/azuresearchbasic) vagy valamelyik hello [szabványos rétegek](search-limits-quotas-capacity.md). Ezek a rétegek számlázható szolgáltatások, a kapacitás lépésekben vásárolt *keresési egységet* (SUs) ahol minden partíció és a replika számít-e egy SU. 

Kevesebb SUs-eredmények arányosan alacsonyabb számlázási használata. Számlázási számára van, amíg hello szolgáltatás be van állítva. Ha nem ideiglenesen használ egy szolgáltatás, hello csak úgy tooavoid számlázási törlésével hello szolgáltatást, és majd újra hozza létre, ha esetleg szükség lenne rá.

> [!Note]
> A szolgáltatás törlése töröl minden rajta. Nem a létesítmény biztonsági mentése az Azure Search belül van, és keresési adatainak visszaállítása maradnak. egy új szolgáltatás a meglévő index tooredeploy, kell futtatja hello használt program toocreate, és töltse be eredetileg. 

## <a name="terminology-partitions-and-replicas"></a>Terminológia: partíciók és replikák
Partíciók és a replikák is az elsődleges erőforrások hello biztonsági egy keresési szolgáltatás.

| Erőforrás | Meghatározás |
|----------|------------|
|*Partíciók* | Index tároló és az I/O biztosít olvasási/írási műveletek (például amikor újraépítését, vagy az index frissítése).|
|*Replikák* | Hello keresési szolgáltatás példánya, elsősorban tooload egyenleg lekérdezési műveletek használt. Minden egyes replikának mindig tartalmazó egy indexet. Ha 12 replikákat, hogy minden hello szolgáltatás betöltött index 12 másolatát.|

> [!NOTE]
> Nincs mód toodirectly módosítására, vagy olyan replikán, futtassa mely Indexek kezelése. Egy példányát minden egyes index minden replikán hello szolgáltatás architektúrájának részét képezi.
>

## <a name="how-tooallocate-partitions-and-replicas"></a>Hogyan tooallocate partíciókat és a replikák
Az Azure Search szolgáltatás eredetileg lefoglalt egy partíciót és egy másodpéldány-erőforrások megadott minimális szintjét. Az rétegek, amelyek támogatják ezt a partíciók növelésével, ha több tároló és az I/O kell, vagy adja hozzá a további replikák nagyobb lekérdezés kötetek vagy jobb teljesítményt Növekményesen módosíthatja számítási erőforrások. Egyetlen szolgáltatás összes munkaterhelések (indexelő és lekérdezések) kell rendelkeznie a megfelelő erőforrásokat toohandle. Munkaterhelésének több szolgáltatás nem tudja tovább.

tooincrease, vagy módosítsa hello kiosztását a replikák és a partíciók, azt javasoljuk, hello Azure-portálon. hello portal érvénybe lépteti a megengedett maximális határértékek alatt maradnak kombinációk vonatkozó korlátozások:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) hello keresési szolgáltatást, majd.
2. A **beállítások**, nyissa meg hello **méretezési** panel és -felhasználási csúszkák tooincrease hello, vagy csökkentse a partíciók és replikák hello száma.

Ha a parancsprogram-alapú vagy a kód-alapú létesítési módszer van szüksége, hello [felügyeleti REST API](https://msdn.microsoft.com/library/azure/dn832687.aspx) egy alternatív toohello portál.

Alkalmazások keresése általában akkor kell további replikák partíciók, mint, különösen akkor, ha a hello szolgáltatás műveletek felé lekérdezés munkaterhelések vannak optimalizálva. a szakasz hello [magas rendelkezésre állású](#HA) azt ismerteti, hogy miért.

> [!NOTE]
> A szolgáltatás üzembe helyezése után frissített tooa nem lehet nagyobb Termékváltozat. Meg kell toocreate hello új réteget egy keresési szolgáltatás, és töltse be újra az indexek. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) szolgáltatás kiépítése segítségre.
>
>

<a id="HA"></a>

## <a name="high-availability"></a>Magas rendelkezésre állás
Könnyű, és viszonylag gyors tooscale fel, mert általában javasoljuk, hogy a kiindulási pont egy partíciót, és egy vagy két replikákat, és ezután lekérdezés kötetként felskálázott készítsen. Sok szolgáltatásának hello Basic vagy S1 rétegek egy partíciót itt elegendő tárhely és i/o (a 15 millió dokumentumot partíciónként).

Lekérdezés munkaterhelések futtatásához elsősorban a replikákon. Ha további átviteli vagy magas rendelkezésre állású, további replikák valószínűleg szüksége lesz.

A magas rendelkezésre állás általános ajánlások a következők:

* A magas rendelkezésre állás csak olvasható munkaterhelések (lekérdezések) két replika
* Három vagy több replikák a magas rendelkezésre állás olvasási/írási munkaterhelések (lekérdezések és indexelő, egyes dokumentumok hozzáadott, frissítése vagy törlése)

Szolgáltatásszint-szerződések (SLA) az Azure Search lekérdezési műveletek és az index frissítések, amelyek hozzáadása, frissítése vagy törlése a dokumentumok célozzák meg.

### <a name="index-availability-during-a-rebuild"></a>Során egy rebuild index elérhetősége

Az Azure Search magas rendelkezésre állású, amely nem tartalmaz, amely az index újraépítése tooqueries és index frissítések vonatkozik. Ha a mező törlése, módosítása adattípust vagy mező átnevezése, szüksége lesz a toorebuild hello index. toorebuild hello index, törölnie kell az hello index, hello index újbóli létrehozása, és töltse be újra a hello adatokat.

> [!NOTE]
> Új mezők tooan Azure Search-index hello index újraépítése nélkül is hozzáadhat. hello új mező hello értéke már hello index található összes dokumentum NULL értékű lesz.

toomaintain index rendelkezésre állási egy Újraépítés alatt, kell rendelkeznie egy eltérő nevű hello index másolatát az azonos név a egy másik szolgáltatást, és adja meg a kódban átirányítása vagy feladatátvételi logika hello hello azonos szolgáltatástól, vagy egy-egy példányát hello index.

## <a name="disaster-recovery"></a>Vészhelyreállítás
Jelenleg nincs vész-helyreállítási beépített mechanizmus. Partíciók vagy replikák hello helytelen stratégiája vész-helyreállítási célkitűzések lenne. hello leggyakrabban használt megközelítés tooadd redundancia hello szolgáltatás szintjén beállítása egy második keresési szolgáltatást, egy másik régióban. Csakúgy, mint a rendelkezésre állási során egy index rebuild, hello átirányítása vagy feladatátvételi logika kell származnia a kódot.

## <a name="increase-query-performance-with-replicas"></a>Növelje a lekérdezési teljesítmény replikával
Lekérdezés-késleltetés értéke azt jelzi, hogy van-e szükség további replikákat. Általában a lekérdezési teljesítmény első lépését tooadd további erőforrás. Replikák hozzáadásakor, további példányokat hello index online toosupport nagyobb lekérdezés munkaterhelések kerülnek, és keresztül hello kérelmek tooload egyenleg hello több replika.

Rögzített becslése / másodperc (QPS) a lekérdezések nem nyújtunk: hello hello bonyolultságától függ lekérdezés lekérdezés és versengő munkaterhelések. Átlagosan Basic vagy S1 termékváltozatok replika készül 15 QPS is szolgáltatás, de az átviteli sebesség magasabb vagy alacsonyabb attól függően, hogy a lekérdezés összetettsége (jellemzőalapú lekérdezések összetettebbek) és a hálózati késés. Azt is fontos, hogy a replikák mindenképpen felveszi méretezés és teljesítmény, de hello eredmény toorecognize nincs szigorúan lineáris: három replikák hozzáadása nem garantálja háromszoros átviteli sebességet.

toolearn QPS, beleértve a QPS becsléséhez a munkaterhelések megközelítések kapcsolatban lásd: [a Search szolgáltatás kezelése](search-manage.md).

## <a name="increase-indexing-performance-with-partitions"></a>A partíciók indexelési teljesítmény növelése
Közel valós idejű adatok frissítést igénylő alkalmazások keresése mint replikák arányosan több partíciót kell. Partíciók hozzáadása között osztja el olvasási/írási műveletek nagy számú számítási erőforrások között. Azt is lehetővé teszi több lemezterületet tárolásához további indexekhez és dokumentumokhoz.

Nagyobb indexek hosszabb tooquery igénybe vehet. Előfordulhat például, hogy minden egyes partíciók növekményes növekedése igényel-e a kisebb, de arányos növelését a replikákat. a lekérdezések és a lekérdezés kötet hello összetettsége fog tényező be, hogy milyen gyorsan be van kapcsolva a lekérdezés-végrehajtás.

## <a name="basic-tier-partition-and-replica-combinations"></a>Az alapszintű csomag: partícióazonosító és másodpéldány kombinációk
Egy alapszintű service pontosan egy partícióval rendelkezik, és akár egy legfeljebb három SUS-t a toothree replikáit. hello csak állítható erőforrás a replikákat. Legalább két replika lekérdezések magas rendelkezésre állásra van szükség.

<a id="chart"></a>

## <a name="standard-tiers-partition-and-replica-combinations"></a>Standard rétegek: partícióazonosító és másodpéldány kombinációk
Az alábbi táblázatban hello SUs szükséges toosupport kombinációk replikák és a partíciók, a tulajdonos toohello 36-SU korlátot, az összes szabványos rétege számára.

|   | **1 partíció** | **2 partíció** | **3 partíció** | **4 partíciók** | **6 partíciók** | **12 partíciók** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 replika** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 replikák** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 replikák** |3 SU |6 SU |9-ES SU |12 SU |18 SU |36 SU |
| **4 replikák** |4 SU |8 SU |12 SU |16 SU |24 SU |N/A |
| **5 replikák** |5 SU |10 SU |15 SU |20 SU |30 SU |N/A |
| **6 replikák** |6 SU |12 SU |18 SU |24 SU |36 SU |N/A |
| **12 replikák** |12 SU |24 SU |36 SU |N/A |N/A |N/A |

SUS-t, a díjszabás és a kapacitás részletesen a hello Azure-webhelyen. További információkért lásd: [díjszabás](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> replikák és a partíciók száma hello egyenlően osztja 12 (pontosabban, 1, 2, 3, 4, 6, 12). Ennek oka az Azure Search előre osztja minden index 12 szilánkok, így az is lehet elosztva egyenlő részre minden partíció. Például ha a szolgáltatás három partíciókkal rendelkezik, és az index létrehozása, mindegyik partíció hello index négy szilánkok fogja tartalmazni. Hogyan index egy Azure Search szilánkok egy megvalósítási részletes, tulajdonos toochange későbbi kiadásai. Bár hello szám 12 ma, nem várt, hogy number tooalways 12 jövőbeli hello lennie.
>
>

## <a name="billing-formula-for-replica-and-partition-resources"></a>A replika és a partíció erőforrások számlázási képlet
hello kiszámítása bizonyos kombinációinál használt hány SUs képlete hello termék a replikák és a partíciók, vagy (R X P = SU). Például három replikák és a három kilenc SUs terheli.

SU onkénti költséget hello rétegben, mint a standard Basic alacsonyabb egység számlázási sebességet határozza meg. Az egyes rétegek található minősíti [díjszabás](https://azure.microsoft.com/pricing/details/search/).
