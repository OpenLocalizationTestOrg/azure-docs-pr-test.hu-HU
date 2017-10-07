---
title: "az Azure importálási/exportálási feladatok aaaRetrieving állapotadatokat |} Microsoft Docs"
description: "Ismerje meg, hogyan tooobtain állapotinformáció a(z) Microsoft Azure Import/Export szolgáltatás feladatok."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 22d7e5f0-94da-49b4-a1ac-dd4c14a423c2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: muralikk
ms.openlocfilehash: cbc35660519573d92f641924ac0025c9e577d69b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retrieving-state-information-for-an-importexport-job"></a>Importálási/exportálási feladatok állapot adatainak lekérése során
Hello hívása [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) művelet tooretrieve információ is importálni és exportálni a feladatokat. visszaadott hello információk a következők:

-   hello hello feladat jelenlegi állapota.

-   hello hozzávetőleges százalékos, hogy minden feladat befejeződött-e.

-   hello minden olyan meghajtó aktuális állapotát.

-   URI-azonosítók a blobok tartalmazó hibanaplókat és részletes naplózási információk (Ha engedélyezve van).

hello alábbi szakaszok ismertetik a hello által visszaadott hello adatok `Get Job` műveletet.

## <a name="job-states"></a>Feladatállapotok
hello tábla és hello állapota az alábbi ábrán ismertetik a hello állapotok életciklusa során keresztül átkerül egy feladatot. hello hello feladat jelenlegi állapota lehet megállapítani a hívó hello `Get Job` műveletet.

![JobStates](./media/storage-import-export-retrieving-state-info-for-a-job/JobStates.png "JobStates")

hello következő táblázatban állapotokat ismerteti, amely egy feladat lehet, hogy továbbítása.

|Feladat állapota|Leírás|
|---------------|-----------------|
|`Creating`|Miután hello feladat Put művelet hívása, egy feladat jön létre és állapotában értéke túl`Creating`. Hello feladat hello közben `Creating` állapotba kerül, hello Import/Export szolgáltatás azt feltételezi, hogy hello meghajtók még nincsenek szállított toohello adatközpont. Egy feladat maradhatnak hello `Creating` állapotban a(z) tootwo hét, amely után a rendszer automatikusan törli hello szolgáltatás be.<br /><br /> Ha hello frissítés Feladattulajdonság művelet meghívja a hello feladat hello közben `Creating` állapotba kerül, hello feladat marad a hello `Creating` kerül, és hello időtúllépési időköz alaphelyzetbe állítása tootwo hét.|
|`Shipping`|A csomag küldje el, miután túl hívja a hello frissítése feladat tulajdonságai művelet frissítés hello hello feladat állapota`Shipping`. hello szállítási állapota beállítható, csak ha hello `DeliveryPackage` (postai szolgáltatója és követési szám) és hello `ReturnAddress` tulajdonságait úgy állították be hello feladat.<br /><br /> hello feladat hello szállítási állapotának mentése tootwo hét marad. Ha két héten átadott és hello meghajtók nem fogadott, hello Import/Export service operátorok értesítést fog kapni.|
|`Received`|Összes fogadott hello data Center, miután hello feladat állapota lesz beállítva, toohello fogadott állapota.|
|`Transferring`|Hello meghajtók hello data Center fogadott, és legalább egy meghajtó már megkezdődött a feldolgozás után hello feladatállapotot lesz beállítva, toohello `Transferring` állapotát. Lásd: hello `Drive States` szakasz alatt részletes információkat.|
|`Packaging`|Után az összes meghajtón befejezte a feldolgozási, hello feladat kerülnek hello `Packaging` végéig hello meghajtók szállított hátsó toohello felhasználói állapot.|
|`Completed`|Miután az összes meghajtó lett szállított hátsó toohello-ügyfelekre, ha hello feladat hiba nélkül befejeződött, majd hello feladat lesz beállítva, toohello `Completed` állapotát. hello feladat automatikusan törölve lesz a hello 90 nap után `Completed` állapotát.|
|`Closed`|Miután az összes meghajtó lett szállított hátsó toohello ügyfél, ha nem történt hiba hello feladat hello feldolgozása során, majd hello feladat lesz beállítva, toohello `Closed` állapotát. hello feladat automatikusan törölve lesz a hello 90 nap után `Closed` állapotát.|

Megszakíthatja a feladatot csak egyes állapotok. A megszakított feladatok kihagyja hello adatok másolása lépést, de egyébként nem megszakították feladatként átmenetek azonos állapot hello következik.

hello következő táblázat ismerteti, hogy minden feladat állapotát, valamint a hello feladat hello hatással a hiba akkor fordul elő hibák.

|Feladat állapota|Esemény|Megoldási / további lépések|
|---------------|-----------|------------------------------|
|`Creating or Undefined`|Egy vagy több meghajtó feladat érkezett, de hello feladat nincs hello `Shipping` állam vagy nincs rekord hello feladat hello szolgáltatásban.|hello Import/Export szolgáltatás műveleti csapata toocontact hello ügyfél toocreate kísérlet, vagy a szükséges információt továbbítható feladat toomove/hello hello feladat frissíteni.<br /><br /> Ha hello műveleti csapata nem toocontact hello vevő két héten belül, akkor hello műveleti csapata tooreturn hello meghajtók kísérli meg.<br /><br /> Hello esemény hello meghajtók nem adható vissza és hello ügyfél nem érhető el, a meghajtók hello biztonságosan megsemmisülnek 90 napban.<br /><br /> Vegye figyelembe, hogy egy feladatot nem lehet feldolgozni, amíg az állapot nem frissül túl`Shipping`.|
|`Shipping`|egy feladat hello csomag nem érkezett meg több mint két hétig.|hello műveleti csapata értesíti hello ügyfél hello hiányzó csomag. Hello az ügyfél válasz alapján, hello műveleti csapata fog hello időköz toowait a hello csomag tooarrive kiterjesztése vagy hello megszakítása.<br /><br /> Hello eseményben tartozó hello ügyfél nem érhető el vagy nem válaszol számított 30 napon belül hello műveleti csapata indít el a művelet toomove hello feladatot hello `Shipping` állapot közvetlenül toohello `Closed` állapotát.|
|`Completed/Closed`|hello meghajtók soha nem hello Válaszcím érhető el, vagy sérült a szállítási (csak tooan exportálási feladat vonatkozik).|Ha hello meghajtók nem érte el a hello Válaszcím, hello ügyfél első hívás hello Get Job művelet kell, vagy ellenőrzés hello feladat állapotát a portál tooensure hello adott hello meghajtók megtörtént. Ha rendelkezik teljesítettek a hello meghajtók, majd hello ügyfél kell forduljon hello szállítási szolgáltató tootry és hello meghajtók keresse meg.<br /><br /> Hello meghajtó sérült szállítás során, ha hello ügyfél toorequest érdemes egy másik exportálási feladat, vagy a letöltési hello hiányzó bináris objektumok.|
|`Transferring/Packaging`|Feladat rendelkezik egy hibás vagy hiányzik a címet.|hello műveleti csapata lesznek elérhetők a toohello kapcsolattartó hello feladat tooobtain hello megfelelő címet.<br /><br /> Hello eseményben tartozó hello ügyfél nem érhető el, hello meghajtók 90 napban biztonságosan megsemmisülnek.|
|`Creating / Shipping/ Transferring`|Csomag szállítási hello olyan meghajtó, amely nem jelenik meg importálni meghajtók toobe hello listáját tartalmazza.|hello további meghajtók nem dolgozható fel, és visszatér toohello ügyfél hello feladat elvégzésekor.|

## <a name="drive-states"></a>A meghajtó állapota
hello táblázat és az alábbi ábrán hello leírják hello életciklusának meghajtók egyéni, akkor átkerül egy importálási vagy exportálási feladat keresztül. Hello aktuális meghajtó állapotát hívó hello olvashatók be `Get Job` műveletet, és tanulmányozza hello `State` hello eleme `DriveList` tulajdonság.

![DriveStates](./media/storage-import-export-retrieving-state-info-for-a-job/DriveStates.png "DriveStates")

hello következő táblázatban állapotokat ismerteti, hogy a meghajtó esetleg továbbítása.

|Meghajtó állapotát|Leírás|
|-----------------|-----------------|
|`Specified`|Hogy az importálás, ha hello feladat jön létre a hello feladat Put művelet hello kezdeti egy meghajtó állapota hello `Specified` állapotát. Exportálási feladat, a meghajtó nem adható meg hello feladat jön létre, mert hello kezdeti meghajtó állapota hello `Received` állapotát.|
|`Received`|hello meghajtó átmenetek toohello `Received` állapot, amikor hello Import/Export szolgáltatás operátor feldolgozta-e, hogy az importálás vállalati szállítási hello érkezett hello meghajtókat. Az exportálási feladat, hello kezdeti meghajtó állapota hello `Received` állapotát.|
|`NeverReceived`|hello meghajtó áthelyezi toohello `NeverReceived` állapot, amikor hello feladat csomag megérkeznek, de hello csomag nem tartalmaz hello meghajtó. A meghajtó is áthelyezheti a állapotba, ha két héten óta hello szolgáltatás hello szállítási adatot kapott, de hello csomag még nem érkezett meg hello adatközpont lett.|
|`Transferring`|A meghajtó-bA lesznek áthelyezve toohello `Transferring` állapot, amikor hello szolgáltatás megkezdi az hello meghajtó tooWindows Azure Storage tootransfer adatait.|
|`Completed`|A meghajtó-bA lesznek áthelyezve toohello `Completed` állapot, amikor hello szolgáltatás sikeresen átadta összes hello adatok hibásak.|
|`CompletedMoreInfo`|A meghajtó-bA lesznek áthelyezve toohello `CompletedMoreInfo` állapot, amikor hello szolgáltatás észlelt kapcsolatos néhány problémát ismertetünk az adatok másolásának származhatnak vagy toohello meghajtó. hello információk között szerepelhet hibák, figyelmeztetések és információs üzenetek blobok felülírására.|
|`ShippedBack`|hello meghajtó áthelyezi toohello `ShippedBack` teljesítettek a hello data center vissza toohello Válaszcím állapothoz.|

hello következő táblázat hello meghajtó hiba állapotok és az egyes állapotokhoz végrehajtott hello műveleteket.

|Meghajtó állapotát|Esemény|Megoldás / a következő lépés|
|-----------------|-----------|-----------------------------|
|`NeverReceived`|A jelölés szerint meghajtót `NeverReceived` (mert nem érkezett hello feladat szállítási részeként) egy másik szállítási érkezik.|hello műveleti csapata áthelyezi hello meghajtó toohello `Received` állapotát.|
|`N/A`|A meghajtó, amely nem része semmilyen feladatot hello adatközpont egy másik feladat részeként érkezik.|hello meghajtó egy extra meghajtóként lesz megjelölve, és az eredmény toohello ügyfél hello eredeti csomaghoz társított hello feladat elvégzésekor.|

## <a name="faulted-states"></a>Hibás állapotok
Ha egy feladat vagy a meghajtó nem várt élettartama során általában tooprogress, hello feladat vagy a meghajtó kerül egy `Faulted` állapotát. Ezen a ponton hello műveleti csapata kapcsolatba fog lépni hello felhasználói e-mailek vagy a telefon. Hello probléma megoldása után hello feladat hibát jelzett, vagy kívül hello megnyílik meghajtó `Faulted` állapot és a hello áthelyezett megfelelő állapotot.

## <a name="next-steps"></a>Következő lépések

* [Hello Import/Export szolgáltatás REST API használatával](storage-import-export-using-the-rest-api.md)
