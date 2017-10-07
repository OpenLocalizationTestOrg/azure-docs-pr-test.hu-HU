---
title: "automatikus skálázás aaaBest eljárásai |} Microsoft Docs"
description: "Automatikus skálázás minták Azure Web Apps, a virtuálisgép-méretezési készletek és a Cloud Services csomag"
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 9fa2b94b-dfa5-4106-96ff-74fd1fba4657
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: ancav
ms.openlocfilehash: eb731c15e440af93a2675210583878814d0d8818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-autoscale"></a>Ajánlott eljárások az automatikus méretezéshez
Ez a cikk útmutatást ad az ajánlott eljárások tooautoscale az Azure-ban. Az Azure a figyelő automatikus skálázás csak túl vonatkozik[virtuálisgép-méretezési csoportok](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Felhőszolgáltatások](https://azure.microsoft.com/services/cloud-services/), és [App Service - webalkalmazások](https://azure.microsoft.com/services/app-service/web/). Más Azure-szolgáltatások különböző méretezési módszer használatára.

## <a name="autoscale-concepts"></a>Automatikus skálázás fogalmak
* Egy erőforrás csak rendelkezhet *egy* automatikus skálázási beállítás
* Az automatikus skálázási beállítás egy vagy több profilok és az egyes profilok lehet egy vagy több automatikus skálázási szabályok.
* Az automatikus skálázási beállítás méretezi példányok vízszintesen, amely *kimenő* hello példányok növelésével és *a* hello példányok számának csökkentésével.
  Az automatikus skálázási beállítás tartozik egy maximális, minimális és példányok alapértelmezett értéke.
* Az automatikus skálázási feladat, mindig hello a hozzá kapcsolódó metrika tooscale ellenőrzésével, elért hello kibővített vagy méretezési a beállított küszöbértéket. Listáját megtekintheti a mérőszámok, hogy az automatikus skálázás méretezheti által a [Azure figyelő automatikus skálázás közös metrikák](insights-autoscale-common-metrics.md).
* Minden küszöbértéke egy példány szintjén kiszámítása. Például "1 példány által végzett méretezési amikor átlagos CPU > 80 %, ha a példányok száma 2", azt jelenti, hogy a kibővített 80 %-nál nagyobb hello átlagos CPU-csomagbeli összes példányra esetén.
* Mindig kapják értesítések e-mailben. Hello tulajdonos, közreműködő és hello célerőforrása olvasói kifejezetten, e-maileket kapni. Mindig is kap egy *helyreállítási* e-mailt automatikus skálázás helyreállít a hibát, és elindítja a szokásos módon működik-e.
* Ön részt tooreceive sikeres skálázási művelet értesítést e-mailek és webhookokkal keresztül.

## <a name="autoscale-best-practices"></a>Automatikus skálázás gyakorlati tanácsok
Használja az automatikus skálázás használata a következő gyakorlati tanácsok hello.

### <a name="ensure-hello-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Győződjön meg arról, hello maximális és minimális értékek eltérőek, és a közöttük egy megfelelő margóval rendelkeznie
Ha a beállítást, amely rendelkezik a minimális = 2, legfeljebb = 2 és hello aktuális példányszám 2, akkor fordulhat elő, nem skálázási művelet. Tartsa meg egy megfelelő margója közötti hello maximális és minimális példányok számát, amely átfogó jellegűek. Automatikus skálázás mindig méretezze át a korlátok között.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Manuális skálázás automatikus skálázás min és max alaphelyzetbe állítása
Manuálisan hello példány tooa számérték fenti frissítésekor vagy alatti hello, hello automatikus skálázás motor automatikusan méretezi hátsó toohello minimális (Ha alacsonyabb) vagy a maximális hello (Ha újabb). Például beállíthatja hello 3. és 6 közötti tartományba esik. Ha egy futó példány, a hello automatikus skálázás motor too3 példányok méretezi a következő futásakor. Ehhez hasonlóan az volna méretezési-a 8 példányok biztonsági too6 a következő futásakor.  Manuális skálázás csak nagyon ideiglenes, kivéve, ha alaphelyzetbe állít egy hello automatikus skálázási szabályok is.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Mindig használja a kibővített és skálázási szabály, amely végrehajtja az növekedés és csökkentése
Hello kombinációja csak egy részét használja, ha automatikus skálázás méretezési modul, amely egyetlen, vagy, hello maximális és minimális, amíg eléri.

### <a name="do-not-switch-between-hello-azure-portal-and-hello-azure-classic-portal-when-managing-autoscale"></a>Nem váltás hello Azure-portál és a klasszikus Azure portálon hello automatikus skálázás kezelése
Felhőalapú szolgáltatásokhoz és alkalmazásszolgáltatások (webalkalmazások) hello Azure portal (portal.azure.com) toocreate használja, és automatikus skálázás beállításainak kezelése. A virtuálisgép-méretezési csoportok használjon PowerShell, a parancssori felületen vagy a REST API toocreate, és kezelheti az automatikus skálázási beállítás. Nem Váltás a klasszikus Azure portálon (manage.windowsazure.com) hello és hello Azure portal (portal.azure.com) automatikus skálázási konfigurációk kezelésekor. hello a klasszikus Azure portálon, és az alapjául szolgáló háttér korlátai. Helyezze át a toohello az Azure portál toomanage automatikus skálázás egy grafikus felhasználói felület használatával. a következők hello toouse hello automatikus skálázás PowerShell, a parancssori felületen vagy a REST API-t (az Azure erőforrás-kezelő) keresztül.

### <a name="choose-hello-appropriate-statistic-for-your-diagnostics-metric"></a>Válassza ki a hello megfelelő statisztika a diagnosztika mérőszám
Diagnosztika metrikáihoz közül választhat *átlagos*, *minimális*, *maximális* és *teljes* , egy metrika tooscale által. hello leggyakoribb statisztikai adat *átlagos*.

### <a name="choose-hello-thresholds-carefully-for-all-metric-types"></a>Válassza ki a hello küszöbértékek gondosan összes mérték típusa
Azt javasoljuk, körültekintően a kibővített és a skála gyakorlati helyzetek alapján különböző küszöbértékek kiválasztása.

A Microsoft *nem javasoljuk* automatikus skálázási beállítások, például az alábbi hello példák hello azonos vagy nagyon hasonló küszöbértékei, és a feltételek:

* 1 növeléséhez a példányok száma, amikor szálak száma < = 600
* 1 csökkentése példányok száma, amikor szálak száma > = 600

Nézzük például milyen vezethet tűnhet félrevezető tooa működésére. Vegye figyelembe a következő feladatütemezési hello.

1. Tegyük fel, a 2 példányok toobegin, és majd hello átlagos példányonként szálak száma nő too625.
2. Automatikus skálázás arányosan kimenő hozzáadja a 3. példányt.
3. A következő azt feltételezik, hogy a hello átlagos szálak száma példány között too575 esik.
4. Skálázás, mielőtt az automatikus skálázási megpróbál tooestimate milyen hello végső állapot lesz, ha a méretezve. Például 575 x 3 (aktuális példányszám) = 1,725 / 2 (ha méretezhető végleges száma) 862.5 szálak =. Ez azt jelenti, hogy automatikus skálázás kellene tooimmediately kibővített újra még azt követően, méretezhető, ha hello átlagos szál száma továbbra is hello azonos vagy akár csak kis mértékben csökken. Azonban ha, akár méretezhető, volna ismételje meg a teljes folyamat hello, ami tooan végtelen ciklus.
5. tooavoid ebben az esetben ("flapping" hívják) automatikus skálázási nem csökkentheti egyáltalán. Ehelyett a használatával kihagyhatja, és reevaluates hello feltétel újra hello tovább idő hello szolgáltatás feladatot hajt végre. Ez sikerült megzavarják sokan, mert automatikus skálázás sikertelen toowork állapotában 575 hello átlagos szálak száma.

Során egy méretezési a becslés "váltakozó állapotú" olyan helyzetekben, a méretezés és a kibővített műveletek folyamatosan hová oda-vissza tervezett tooavoid. A kibővített és az azonos küszöbértékek hello kiválasztásakor vegye figyelembe ezt a viselkedést.

Azt javasoljuk, hogy egy megfelelő margó hello kibővített között, és a küszöbértékek kiválasztása. Tegyük fel fontolja meg a következő jobb szabály kombinációja hello.

* 1 növeléséhez a példányok száma, amikor CPU % > = 80
* 1 csökkentése példányok száma, amikor CPU % < = 60

Ebben az esetben  

1. Tegyük fel, a 2 példányok toostart van.
2. Ha hello átlagos CPU % között példányok too80 kerül, automatikus skálázás méretezi a kimenő hozzáadja egy harmadik példányt.
3. Mostantól azt feltételezi, hogy idő hello CPU keresztül % too60 esik.
4. Az automatikus skálázás tartozó skála a szabály becslése hello végső állapot, mintha tooscale a. Például 60 x 3 (aktuális példányszám) = 180 / 2 (ha méretezhető végleges száma) = 90. Így automatikus skálázás nem skálázási be mert tooscale kibővített azonnal újra kellene. Ehelyett kihagyja skálázás.
5. hello tovább idő automatikus skálázás ellenőrzi, hello CPU toofall too50 továbbra is. Becslése újra - példány 50 x 3 = 150 / 2 példányok = 75, amely 80-as, hello kibővített küszöbérték alatt, a méretezés sikeresen too2 példányát.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Különleges metrikák küszöbértékei skálázás szempontjai
 Például a tároló- és Service Bus-üzenetsorba hossza metrika különleges metrikáihoz a hello küszöbértéke hello kiszolgálónként példányok jelenlegi száma üzenetek átlagos száma. Gondosan válasszon hello hello tartozó küszöbérték. Ez a metrika válassza.

Most azt mutatják be egy példa tooensure a hello viselkedés jobb megértése.

* Példányok növelhető 1 száma szerint, ha a tároló várólista üzenet-száma > = 50
* Példányok csökkentése 1 száma szerint, ha Storage Üzenetsorába száma < = 10

Vegye figyelembe a következő feladatütemezési hello:

1. Nincsenek 2 tároló várólista példányok.
2. Üzeneteket érkezni és hello storage üzenetsorába áttekintéséhez hello számuk beolvassa 50. Ön azt feltételezik, hogy az automatikus skálázási elindul egy kibővíthető műveletet. Azonban ügyeljen arra, hogy a rendszer továbbra is 50/2 = példányonként 25 üzeneteket. Igen kibővített nem történik meg. Hello első kibővített toohappen hello összes üzenet számán hello tároló várólista legyen 100.
3. A következő azt feltételezik, hogy hello összes üzenet számán eléri-e a 100 értéket.
4. A 3. tároló várólistára kerül tooa kibővített művelet miatt.  hello következő kibővített művelet nem történik teljes hello amíg hello várólistában lévő üzenetek száma eléri a 150, mert 150/3 = 50.
5. Most hello hello várólistában lévő üzenetek számának beolvasása kisebb. 3 osztályt hello első skálázási művelet történik, ha hello összes várólistán található összes üzenet egyezzen too30, mert 30/3 = hello méretezési a küszöbérték, példányonként 10 üzeneteket.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Ha az automatikus skálázási beállításban több profilok méretezéshez szempontok
Az automatikus skálázási beállításban választhat egy alapértelmezett profilt, amely minden függőség ütemezés vagy idő nélkül mindig érvényes, vagy választhatja azt egy ismétlődő vagy profil egy meghatározott ideig dátum és idő tartománnyal.

Amikor az automatikus skálázás szolgáltatás feldolgozza azokat, mindig ellenőrzi a sorrend hello:

1. Rögzített dátum profil
2. Ismétlődő profil
3. Alapértelmezett profil ("Always")

Ha egy profil feltétel teljesül, automatikus skálázási nem hello következő profil feltétel alatta ellenőrzése. Automatikus skálázás egyszerre csak egy profil dolgozza fel. Ez azt jelenti, ha azt szeretné, hogy tooalso közé tartoznak az alsó szintű profilból feldolgozási feltétel, meg kell adnia ezeket a szabályokat, valamint aktuális hello-profil.

Tekintsük át, ennek egy példát:

hello az alábbi ábrán az automatikus skálázási beállítás minimális példányok alapértelmezett profillal = 2 és a maximális példányok = 10. Ebben a példában szabályok konfigurált tooscale kibővített lesz, ha hello várólista hello üzenetek száma nagyobb, mint 10 és a skála hello üzenetek száma hello várólistán kevesebb mint 3 esetén. Most hello erőforrás méretezhető 2 és 10 példányai között.

Emellett nincs egy ismétlődő profil hétfő beállítása. A példányok minimális érték = 2 és a maximális példányok = 12. Ez azt jelenti, hétfőn hello első alkalommal automatikus skálázás ellenőrzi a feltétel 2 hello példányszám esetén toohello új legalább 3-as méretezés. Mindaddig, amíg az automatikus skálázás továbbra is fennáll, ez a profil feltétel egyezik (hétfő) toofind, akkor csak feldolgozza hello CPU-alapú scale-out/a szabályok konfigurálva ehhez a profilhoz. Most azt nem ellenőrzi az hello várólistájának hossza. Azonban ha is hello várólista hossza feltétel toobe be van jelölve, bele kell foglalni hello alapértelmezett profilból ezeket a szabályokat, valamint a hétfő profil.

Hasonlóképpen automatikus skálázás hátsó toohello alapértelmezett profil vált, ha először ellenőrzi, ha hello minimális és maximális feltételek teljesülnek-e. Ha hello több példányban hello időpontban 12, méretezés too10, az engedélyezett maximális hello hello alapértelmezett profilt.

![automatikus skálázási beállításokat](./media/insights-autoscale-best-practices/insights-autoscale-best-practices-2.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Több szabály profil konfigurálásakor skálázás szempontjai
Előfordulhatnak olyan esetek, ahol előfordulhat, hogy tooset több szabály profilban. a következő automatikus skálázási szabályok hello használata szolgáltatások használatával, amikor több szabályok be vannak állítva.

A *horizontális felskálázás*, automatikus skálázás fut, ha minden szabály teljesül.
A *méretezési a*, automatikus skálázás megkövetelése minden szabály teljesül toobe.

tooillustrate, azt feltételezik, hogy rendelkezik-e a következő 4 automatikus skálázási szabályok hello:

* Ha a CPU < 30 %, méretezési-1
* Ha memória < 50 %-kal, méretezési-1
* Ha a CPU-> 75 %, kibővített 1
* Ha memória > 75 %, kibővített 1

Majd hello kövesse megy végbe:

* Ha CPU 76 % és memória 50 %-át, hogy a kibővített.
* Ha CPU 50 %-át, és memória 76 % azt kibővített.

A hello, ugyanakkor, ha a CPU 25 % pedig memória 51 % automatikus skálázás does **nem** méretezési a. Sorrendben tooscale-Processzor kell 29 % és a memória 49 %.

### <a name="always-select-a-safe-default-instance-count"></a>Mindig jelölje be a biztonságos alapértelmezett példányszám
automatikus skálázás méretezi a szolgáltatás toothat száma, amikor nem érhetők el a metrikák hello alapértelmezett példányszám fontos. Ezért válasszon egy alapértelmezett példányok száma, amelyek a munkaterhelések biztonságos.

### <a name="configure-autoscale-notifications"></a>Értesítések automatikus skálázás konfigurálása
Automatikus skálázás értesíti hello rendszergazdák és közreműködő szerepkörrel rendelkező személyek hello erőforrás e-mailben Ha hello a következő feltételek bármelyike teljesül:

* automatikus skálázás szolgáltatás tootake művelet sikertelen lesz.
* Az automatikus skálázás szolgáltatás toomake méretezési döntést metrikák nem érhetők el.
* Adatok gyűjtése le elérhető (helyreállítási) újra toomake méretezési döntést hoznak.
  Ezenkívül toohello feltételek fent, konfigurálhatja sikeres skálázási műveletekhez értesítést kap e-mailben vagy webhook értesítések tooget.
  
Az tevékenységnapló riasztási toomonitor hello állapotát hello automatikus skálázás motor is használható. Az alábbiakban példákat túl[hozzon létre egy tevékenység napló riasztási toomonitor az előfizetés minden automatikus skálázási motor műveletek](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert) vagy túl[hozzon létre egy tevékenység napló riasztási toomonitor összes sikertelen automatikus skálázás skála / a műveletek kiterjesztése az előfizetés](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

## <a name="next-steps"></a>Következő lépések
- [Hozzon létre egy tevékenység napló riasztási toomonitor az előfizetés minden automatikus skálázási motor műveleteket.](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Hozzon létre egy tevékenység napló riasztási toomonitor összes sikertelen automatikus skálázás skála / műveleteket az előfizetés kiterjesztése](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)
