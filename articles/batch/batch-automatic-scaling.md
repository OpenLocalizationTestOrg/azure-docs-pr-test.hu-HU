---
title: "aaaAutomatically méretezési számítási csomópontok az Azure Batch-készlet |} Microsoft Docs"
description: "Enable automatikus méretezésének egy felhőalapú készlet toodynamically beállítása hello hello készlet számítási csomópontjainak számát."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: multiple
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b6d1e0c5d8e0e56e15a4d3588150f2466a689f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-automatic-scaling-formula-for-scaling-compute-nodes-in-a-batch-pool"></a>Hozzon létre egy méretezhetővé válik a Batch-készlet számítási csomópontjainak automatikus méretezési képlet

Az Azure Batch automatikusan át tudja méretezni készletek meghatározott paraméterei alapján. Az automatikus skálázás kötegelt dinamikusan kerülnek be a csomópontok tooa készlet feladat igények növekedése, és eltávolítja a számítási csomópontok, mivel csökkentik. A kötegelt alkalmazás által használt csomópontok száma hello automatikusan módosításával időt és pénzt is mentheti. 

Automatikus skálázás a számítási csomópontok készletét társítja azt engedélyezi egy *automatikus skálázás képlet* megadhat. hello Batch szolgáltatás használja hello automatikus skálázás képlet toodetermine hello számítási csomópontok száma, amelyek a szükséges tooexecute a terhelést. Számítási csomópontok lehet, hogy dedikált csomópontok vagy [alacsony prioritású csomópontok](batch-low-pri-vms.md). Kötegelt tooservice metrikák összegyűjtött adatokon rendszeresen válaszol. A metrikák adatok használatával, kötegelt módosítja a képlet alapján és konfigurálható időközönként hello készlet számítási csomópontjai hello száma.

A tárolókészlet létrehozásakor vagy meglévő címkészlet automatikus skálázás engedélyezéséhez. Az automatikus skálázás beállítása készletben meglévő képlet is módosíthatja. Kötegelt lehetővé teszi a képletekben toopools és toomonitor hello állapotának automatikus skálázás hozzárendelése előtt fut tooevaluate.

Ez a cikk ismerteti, amelyek hello különféle entitásokat, amelyek az automatikus skálázás képletek, beleértve a változók, operátorok, műveletek és funkciók alkotják. Arról lesz szó hogyan tooobtain különböző számítási erőforrás és a feladat metrikák kötegelt belül. Ezek a készlet csomópontok száma alapuló erőforrás-használat és a feladat állapota metrikák tooadjust is használhatja. Majd ismerteti hogyan hello a képlet tooconstruct és automatikus méretezésének készlet használatával is lehetővé teszik, hogy Batch REST és a .NET API-k. Végül azt befejezéséhez néhány példa képletek.

> [!IMPORTANT]
> Batch-fiók létrehozásakor megadhatja a hello [fiókkonfiguráció](batch-api-basics.md#account), amely meghatározza, hogy készletek foglal le a Batch szolgáltatás az előfizetéshez (hello alapértelmezett), vagy a felhasználó az előfizetéshez. A Batch-fiók hello alapértelmezett Batch szolgáltatás konfigurációs hozta létre, ha a fiók használható a feldolgozás magok maximális száma korlátozott tooa. hello Batch szolgáltatás méretezi a számítási csomópontok csak másolatot toothat core korlátot. Emiatt hello Batch szolgáltatás nem jut el az automatikus skálázás képlet által meghatározott számítási csomópontok száma hello cél. Lásd: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md) információk megtekintéséhez és a fiók kvótákat növelését.
>
>Hello felhasználói előfizetési konfigurációval létrehozta a fiókot, ha a fiók hello előfizetés hello magkvótája megosztja. További információkért lásd [az Azure-előfizetésekre és -szolgáltatásokra vonatkozó korlátozásokat, kvótákat és megkötéseket](../azure-subscription-service-limits.md) ismertető témakör [a virtuális gépek korlátaira](../azure-subscription-service-limits.md#virtual-machines-limits) vonatkozó részét.
>
>

## <a name="automatic-scaling-formulas"></a>Automatikus méretezési képletek
Az automatikus méretezési képlete karakterlánc-értéke megadhat egy vagy több utasítást tartalmaz. hello automatikus skálázás képlet hozzá van rendelve a tooa készlet [autoScaleFormula] [ rest_autoscaleformula] elem (Batch REST) vagy [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] a tulajdonság (Batch .NET). hello Batch szolgáltatás használ a számítási csomópontok képlet toodetermine hello cél száma hello készletben hello következő intervallum feldolgozása. hello képlet karakterlánc nem haladhatja meg a 8 KB-os, mentése too100 utasításokat, amelyek pontosvesszővel elválasztva, és tartalmazhat sortörést és megjegyzéseket is tartalmazhat.

Az eltolásokat tekintheti automatikus méretezési képletek, egy kötegelt automatikus skálázás "nyelv". Képletadat kimutatásai, szabad formátumú kifejezések szolgáltatás által definiált változókat (hello Batch szolgáltatás által beállított változók) és a felhasználó által definiált változókat (Ön által meghatározott változók) is tartalmazhatja. Azok a feladatra, beépített típusok, a kezelők és a funkciók használatával, ezeket az értékeket különféle műveleteket végezhet. Például egy utasítás is igénybe vehet a következő képernyő hello:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Képletek általában több utasítás műveleteket értékek, amelyek akkor kapja meg az előző utasítások tartalmaznak. Például azt szereznie értéket `variable1`, tooa függvény toopopulate átadni `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Vegye fel az automatikus skálázás képlet tooarrive a számítási csomópontok cél számú ezekről az utasításokról. Dedikált csomópontok és alacsony prioritású csomópont is saját tárolóbeállítások, így megadhatja az egyes csomópont target. Az automatikus skálázás képlet tartalmazhat egy célértéke dedikált csomópontok, alacsony prioritású csomópont target értéket vagy mindkettőt.

csomópontok száma hello cél lehet magasabb, alsó, vagy hello ugyanaz, mint az adott típusú hello készletben csomópontok hello aktuális száma. Kötegelt a készlet automatikus skálázás képlet kiértékelése adott időközönként (lásd: [automatikus skálázás intervallumok](#automatic-scaling-interval)). Kötegelt módosítja hello cél hello gyűjtő toohello száma az automatikus skálázás képlet meghatározó értékelési hello időpontban csomópontjának típusonkénti darabszámot.

### <a name="sample-autoscale-formula"></a>Minta automatikus skálázás képlet

Íme egy példa az automatikus skálázás úgy, hogy a legtöbb forgatókönyvhöz módosított toowork lehet. változók hello `startingNumberOfVMs` és `maxNumberofVMs` hello példa képlet módosított tooyour igényeit is lehet. A képlet dedikált csomópontok méretezi, de lehet módosított tooapply tooscale alacsony prioritású csomópontok is. 

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

Az automatikus skálázás képlettel hello készlet először hozza létre egyetlen virtuális gépen. Hello `$PendingTasks` metrika fut, vagy az aszinkron feladatok hello számát határozza meg. hello képlet hello átlagos száma függőben lévő feladatok talál hello utolsó 180 másodperc, és beállítja a hello `$TargetDedicatedNodes` változó ennek megfelelően. hello képlet biztosítja, hogy soha nem dedikált csomópontok hello cél száma meghaladja a 25 virtuális gépek. Új feladatok be, mivel hello készlet automatikusan nő. Feladat befejeződött virtuális gépek szabad egyenként válik, és hello automatikus skálázás képlet zsugorítja hello készlet.

## <a name="variables"></a>Változók
Mindkét használhatja **szolgáltatás által definiált** és **felhasználói** az automatikus skálázás képletekben változók. a Batch szolgáltatás toohello beépített hello szolgáltatás által definiált változókat. Néhány szolgáltatás által definiált változókat írható-olvasható, és csak olvasható. Felhasználói változók az alábbiak változókat, amelyek adhat meg. Hello előző részben, hello példa képletben `$TargetDedicatedNodes` és `$PendingTasks` szolgáltatás által definiált változók. Változók `startingNumberOfVMs` és `maxNumberofVMs` felhasználói változók.

> [!NOTE]
> Szolgáltatás által definiált változókat mindig előzi dollárjelet ($). A felhasználói változók hello dollárjel nem kötelező megadni.
>
>

a következő táblák hello mindkét írható-olvasható, és csak olvasható változók, amelyeket a Batch szolgáltatás hello megjelenítése.

Get, és adja meg a szolgáltatás által definiált változók értékeit hello számítási csomópontok száma toomanage hello a készletben:

| Olvasási és írási szolgáltatás által definiált változó | Leírás |
| --- | --- |
| $TargetDedicatedNodes |hello cél száma dedikált számítási csomópontjain hello készlet. dedikált csomópontok száma hello cél van megadva, mert a készlet nem lehetséges, hogy mindig elérése szükséges hello csomópontok száma. Például ha dedikált csomópontok száma hello cél módosítása előtt hello automatikus skálázás értékelésével készlet elérte hello kezdeti cél, majd hello készlet nem érte el hello cél. <br /><br /> Egy fiók, létre hello Batch szolgáltatás konfigurációs készlet nem érhetik el a célértéket Ha hello cél meghaladja a kötegelt fiók csomópont vagy core kvótát. Egy fiók, létre hello felhasználói előfizetés konfigurációs készlet nem érhetik el a célértéket Ha hello cél meghaladja hello megosztott magkvótája hello előfizetés.|
| $TargetLowPriorityNodes |alacsony prioritású hello cél száma számítási csomópontjain hello készlet. alacsony prioritású csomópontok száma hello cél van megadva, mert a készlet nem lehetséges, hogy mindig elérése szükséges hello csomópontok száma. Például ha alacsony prioritású csomópontok száma hello cél módosítása előtt hello automatikus skálázás értékelésével készlet elérte hello kezdeti cél, majd hello készlet nem érte el hello cél. Egy készlet is nem érhetik el a célértéket Ha hello cél meghaladja a kötegelt fiók csomópont vagy core kvóta. <br /><br /> Alacsony prioritású számítási csomóponton további információkért lásd: [alacsony prioritású virtuális gépek használata a kötegelt (előzetes verzió)](batch-low-pri-vms.md). |
| $NodeDeallocationOption |hello művelet, amikor a számítási csomópontok el lesznek távolítva a készletben. Lehetséges értékek:<ul><li>**requeue**--feladatok azonnal leáll, és úgy, hogy azok újraütemezte helyezi őket újra a feladat-várólistán hello.<li>**Állítsa le**--feladatok azonnal leáll, és eltávolítja azokat a hello feladat-várólistán.<li>**taskcompletion**--vár a jelenleg futó toofinish feladatokat, majd eltávolítja a hello csomópont hello készletből.<li>**retaineddata**– hello készletből hello csomópont eltávolítása előtt tisztítani hello helyi feladat megőrzi lévő összes adat hello csomópont toobe vár.</ul> |

A szolgáltatás által definiált változókat toomake módosításának hello kötegelt szolgáltatásból metrikák alapuló hello érték is beolvasásához:

| Csak olvasható szolgáltatás által definiált változó | Leírás |
| --- | --- |
| $CPUPercent |a processzorhasználat aránya átlagos hello. |
| $WallClockSeconds |felhasznált másodperces hello számát. |
| $MemoryBytes |hello átlagos száma mérete (MB) használt. |
| $DiskBytes |hello hello helyi lemezeken használt gigabájt átlagos száma. |
| $DiskReadBytes |hello olvasott bájtok számát. |
| $DiskWriteBytes |hello írt bájtok száma. |
| $DiskReadOps |lemezolvasási műveletek száma hello végre. |
| $DiskWriteOps |hello végrehajtott írási lemez műveletek száma. |
| $NetworkInBytes |hello bejövő bájtok száma. |
| $NetworkOutBytes |küldött bájtok száma hello. |
| $SampleNodeCount |a számítási csomópontok száma hello. |
| $ActiveTasks |hello a száma, amelyek készen tooexecute, de még nem végrehajtása feladatok. hello $ActiveTasks száma minden feladatokat, amelyek hello aktív állapotban, és amelynek függőségek teljesülnek tartalmazza. Minden feladatot hello aktív állapotban, de amelynek függőségek nem teljesülnek hello $ActiveTasks száma nem tartoznak.|
| $RunningTasks |feladatok fut. hello száma. |
| $PendingTasks |$ActiveTasks és $RunningTasks hello összege. |
| $SucceededTasks |hello számos feladatot, amely sikeresen befejeződött. |
| $FailedTasks |a sikertelen feladatok száma hello. |
| $CurrentDedicatedNodes |hello aktuális száma dedikált számítási csomópontjain. |
| $CurrentLowPriorityNodes |alacsony prioritású hello aktuális száma számítási csomópontok, beleértve azoknak a fürtöknek, pre-empted törölték. |
| $PreemptedNodeCount | hello készletben pre-empted állapotban lévő csomópontok hello száma. |

> [!TIP]
> hello csak olvasható, a szolgáltatás által meghatározott hello előző táblázatban látható értékek *objektumok* társított minden egyes tooaccess adatok különböző módszereket biztosító. További információkért lásd: [szerezze be a mintaadatokat](#getsampledata) című cikkben.
>
>

## <a name="types"></a>Típusok
Ezek a típusok támogatottak a képlet:

* Dupla
* doubleVec
* doubleVecList
* Karakterlánc
* Timestamp típusú--időbélyeg egy összetett struktúra, amely tartalmazza a következő tagok hello:

  * Év
  * a hónap (1-12)
  * a nap (1-31)
  * milyen napra esik (hello formátumban szám; például hétfő 1)
  * óra (24 órás számformátumú; például 13 azt jelenti, hogy 1 óra)
  * a perc (00 és 59 közötti)
  * második (00 és 59 közötti)
* TimeInterval

  * TimeInterval_Zero
  * TimeInterval_100ns
  * TimeInterval_Microsecond
  * TimeInterval_Millisecond
  * TimeInterval_Second
  * TimeInterval_Minute
  * TimeInterval_Hour
  * TimeInterval_Day
  * TimeInterval_Week
  * TimeInterval_Year

## <a name="operations"></a>Műveletek
Ezeket a műveleteket a hello előző szakaszban felsorolt hello típusok engedélyezettek.

| Művelet | Támogatott operátorok | Eredménytípus |
| --- | --- | --- |
| kettős *operátor* dupla |+, -, *, / |Dupla |
| kettős *operátor* timeinterval |* |TimeInterval |
| doubleVec *operátor* dupla |+, -, *, / |doubleVec |
| doubleVec *operátor* doubleVec |+, -, *, / |doubleVec |
| TimeInterval *operátor* dupla |*, / |TimeInterval |
| TimeInterval *operátor* timeinterval |+, - |TimeInterval |
| TimeInterval *operátor* időbélyeg |+ |időbélyeg |
| Timestamp típusú *operátor* timeinterval |+ |időbélyeg |
| Timestamp típusú *operátor* időbélyeg |- |TimeInterval |
| *operátor*dupla |-, ! |Dupla |
| *operátor*timeinterval |- |TimeInterval |
| kettős *operátor* dupla |<, <=, ==, >=, >, != |Dupla |
| karakterlánc *operátor* karakterlánc |<, <=, ==, >=, >, != |Dupla |
| Timestamp típusú *operátor* időbélyeg |<, <=, ==, >=, >, != |Dupla |
| TimeInterval *operátor* timeinterval |<, <=, ==, >=, >, != |Dupla |
| kettős *operátor* dupla |&&, &#124;&#124; |Dupla |

Dupla típusú értékként Ternáris operátorral tesztelése során (`double ? statement1 : statement2`), nem nulla van **igaz**, és nulla **hamis**.

## <a name="functions"></a>Functions
Ezek előre definiált **funkciók** toouse egy automatikus méretezési képlet definiálásakor érhetők el.

| Függvény | Visszatérési típusa | Leírás |
| --- | --- | --- |
| AVG(doubleVecList) |Dupla |Beolvasása hello hello doubleVecList szereplő összes érték átlagos értékét. |
| len(doubleVecList) |Dupla |Beolvasása hello hello doubleVecList létrehozott hello vektor hosszának. |
| LG(Double) |Dupla |2 értéket adja vissza hello napló alapszintű hello a dupla. |
| LG(doubleVecList) |doubleVec |Hello component-wise napló hello doubleVecList alap 2 adja vissza. Egy vec(double) hello paraméter explicit módon kell átadni. Ellenkező esetben hello dupla lg(double) verzió feltételezi. |
| ln(Double) |Dupla |Beolvasása hello dupla hello természetes naplóját. |
| ln(doubleVecList) |doubleVec |Hello component-wise napló hello doubleVecList alap 2 adja vissza. Egy vec(double) hello paraméter explicit módon kell átadni. Ellenkező esetben hello dupla lg(double) verzió feltételezi. |
| log(Double) |Dupla |Visszaadja hello napló hello alap 10 dupla. |
| log(doubleVecList) |doubleVec |Hello component-wise napló hello doubleVecList alap 10 adja vissza. Egy vec(double) hello egyetlen dupla paraméter explicit módon kell átadni. Ellenkező esetben hello dupla log(double) verzió feltételezi. |
| Max(doubleVecList) |Dupla |Beolvasása hello hello doubleVecList maximális értéket. |
| Min(doubleVecList) |Dupla |Beolvasása hello hello doubleVecList minimális értéket. |
| NORM(doubleVecList) |Dupla |Vissza a két-alapértelmezetté hello vektor hello doubleVecList létrehozott hello. |
| a PERCENTILIS (doubleVec v, dupla p) |Dupla |Beolvasása hello PERCENTILIS elem hello vektor v. |
| rand() |Dupla |Egy véletlenszerű értéke 0,0 és 1,0 között. |
| Range(doubleVecList) |Dupla |Hello doubleVecList hello hello minimális és maximális értékek különbségét adja vissza. |
| STD(doubleVecList) |Dupla |Beolvasása hello minta szórásának hello doubleVecList hello értékeit. |
| Stop() | |Leállítja a hello automatikus skálázás kifejezés kiértékelése. |
| Sum(doubleVecList) |Dupla |Beolvasása hello hello doubleVecList összetevői összes hello összege. |
| idő (dateTime karakterlánc = "") |időbélyeg |Ha adja vissza hello időbélyegzőjét hello aktuális idő paraméterek át lettek adva, vagy hello hello dátum/idő karakterlánc időbélyegzőjét, ha átadva. Támogatott dátum és idő formátumok a következők: W3C-DTF és RFC 1123. |
| val (doubleVec v, dupla i) |Dupla |Hello tényező, amely helyén i vektoros v, a nulla kezdődő indexű hello értékét adja vissza. |

Egy listát, amelynek argumentuma fogadhatnak hello függvények hello előző táblázatban ismertetett. hello vesszővel elválasztott lista bármilyen kombinációját *dupla* és *doubleVec*. Példa:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

Hello *doubleVecList* értéke egyetlen konvertált tooa *doubleVec* kiértékelése előtt. Például ha `v = [1,2,3]`, majd hívja `avg(v)` egyenértékű toocalling van `avg(1,2,3)`. Hívása `avg(v, 7)` egyenértékű toocalling van `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Szerezze be a mintaadatokat
Hello Batch szolgáltatás által biztosított metrikákat adatok (minták) automatikus skálázási hajtanak végre műveleteket. A képlet nő, vagy zsugorítja a készlet méretét, amelyek hello szolgáltatás megszerzi hello értékek alapján. hello szolgáltatás által definiált változókat leírt korábban olyan objektumok, amelyek különböző módszereket biztosítanak a társított objektumokat, amelyek tooaccess vonatkozó adatokat. Például hello következő kifejezés mutatja egy kérelem tooget hello utolsó öt percen CPU-használat:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Módszer | Leírás |
| --- | --- |
| GetSample() |Hello `GetSample()` metódus egy adatok minták-vektorát adja vissza.<br/><br/>Egy minta érték metrikák adatok érdemes 30 másodperc. Más szóval minták akkor kapja meg, 30 másodperces. Azonban az alábbi esetekben, amikor egy minta gyűjt, és elérhető tooa képlet késleltetés van. Mint ilyen nem minden mintákat egy adott időszakra vonatkozóan egy képlettel értékelésre érhetők el.<ul><li>`doubleVec GetSample(double count)`<br/>Minták tooobtain hello legutóbbi minták összegyűjtött hello számát adja meg.<br/><br/>`GetSample(1)`az utolsó elérhető minta hello adja vissza. A metrikák, például `$CPUPercent`, azonban ez nem használható mert lehetetlen tooknow *amikor* hello minta gyűjtötte a program. Előfordulhat, hogy friss, vagy a rendszer problémák miatt előfordulhat, hogy sokkal régebbi. Érdemes az ilyen esetekben toouse egy adott időintervallumban alább látható módon.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Adja meg vagy időszakon minta adatgyűjtést. Másik lehetőségként azt is, amely hello elérhetőnek kell lennie a minták hello százalékos kért időkereten belül.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`20 minták alakítanák vissza, ha az elmúlt 10 perc hello összes mintát hello CPUPercent előzmények léteznek. Ha a előzmények hello utolsó perce nem volt elérhető, azonban csak 18 minták akkor adja vissza. Ebben az esetben:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`fognak működni, mert csak 90 százalékos hello minták érhetők el.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`járnak.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Adatgyűjtés a kezdési idő és a befejezési idő időkeretet határozza meg.<br/><br/>Fent említett nincs amikor minta gyűjt, és elérhető tooa képlet késleltetés. Hello használatakor vegye figyelembe az a késleltetés `GetSample` metódust. Lásd: `GetSamplePercent` alatt. |
| GetSamplePeriod() |Egy korábbi minta adathalmaz végrehajtott minták hello dátumtartományt ad vissza. |
| Count() |Beolvasása hello hello metrika előzmények minták száma. |
| HistoryBeginTime() |Beolvasása hello hello legrégebbi rendelkezésre álló adatok minta hello metrika időbélyegzőjét. |
| GetSamplePercent() |Értéket ad vissza egy adott időintervallumban a rendelkezésre álló minták százalékos hello. Példa:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Mivel hello `GetSample` módszer nem jár sikerrel, ha a visszaadott minták hello aránya nem éri el hello `samplePercent` megadott, használhat hello `GetSamplePercent` metódus toocheck első. Majd egy másik műveletet hajthatnak végre, ha elegendő minták léteznek, Leállítás, hello automatikus méretezési értékelése nélkül. |

### <a name="samples-sample-percentage-and-hello-getsample-method"></a>Minták, a minta százalékos és hello *GetSample()* módszer
hello alapvető működéséhez, az automatikus skálázás képlet tooobtain tevékenység- és metrika értékét, majd állítsa be a készlet méretét az adatok alapján. Mint ilyen akkor fontos toohave pontosan ismeri a automatikus skálázás képletek hogyan működnek együtt a metrikai adatok (minták).

**Példák**

hello Batch szolgáltatás rendszeres időközönként a tevékenység- és mérőszámok mintát vesz igénybe, és elérhető tooyour automatikus skálázás képletek lehetővé teszi. Ezeket a mintákat hello Batch szolgáltatás által 30 másodpercenként rögzítését. Van azonban általában a késleltetés között, ha ezek a minták rögzítve, és elérhetővé válnak túl (és tudja olvasni.) az automatikus skálázás formulákat. Emellett toovarious tényezőkkel, például hálózati vagy egyéb infrastrukturális problémára, miatt minták előfordulhat, hogy nem rögzíti egy adott időszakban.

**A minta százalékos aránya**

Amikor `samplePercent` toohello átadása `GetSample()` metódus vagy hello `GetSamplePercent()` metódus lehívásra kerül, _százalékos_ hivatkozik tooa összehasonlítása hello száma minták hello Batch szolgáltatás által rögzített és hello a száma, amelyek a rendelkezésre álló tooyour automatikus skálázás képlet minták.

Nézzük timespan érték 10 perc példaként. Minták tárolja, 30 másodperces timespan érték 10 perc belül, mert hello maximális kötegelt rögzített minták száma 20 minták (2 / perc) lehet. Előfordulhat azonban, miatt toohello hello jelentéskészítő mechanizmus és más olyan problémák Azure-ban rejlő késését, nem csak 15 mintákat, amelyek a rendelkezésre álló tooyour automatikus skálázás képlet olvasását. Így például, hogy 10 perc alatt 75 %-a hello rögzített minták száma lehet elérhető tooyour képlet.

**GetSample() és minta tartományok**

Az automatikus skálázás képletek folyamatos toobe növekvő, és a készletek zsugorítását &mdash; csomópontok hozzáadása vagy eltávolítása a csomópontok. Csomópontok pénz költségeket, mert a képlet egy intelligens módot használó elegendő adatot alapuló elemzési tooensure kívánt. Ezért azt javasoljuk, hogy használja-e a trendek-elemzés a képletekben. Ez a típus növekszik, és az összegyűjtött minták alapján készletek zsugorítja.

Igen, használjon toodo `GetSample(interval look-back start, interval look-back end)` tooreturn vektor minták:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Hello sorban fölött kötegelt kiértékelésekor értékek vektor vissza minták számos. Példa:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Miután hello vektoros minták összegyűjtött használhatók funkciók, például a `min()`, `max()`, és `avg()` tooderive jelentéssel bíró értékeinek hello gyűjtött tartományon.

A fokozott biztonság érdekében beállíthatja egy kiértékelési toofail Ha kisebb, mint egy bizonyos minta százalék érhető el egy adott időszakra vonatkozóan. Ha egy kiértékelési toofail kényszerítéséhez utasította további kötegelt toocease hello képlet értékelése hello megadott százalékos minták nem áll rendelkezésre. Ebben az esetben nem történik változás toohello készletméretet. toospecify szükséges százalékos arányában minták hello értékelési toosucceed, adja meg azt, a harmadik paraméter túl hello`GetSample()`. Itt 75 százalékával minták követelményt van megadva:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Mivel a minta rendelkezésre állási késés előfordulhat, fontos tooalways adjon meg egy időtartományt a következő megjelenését visszaírt kezdő időpontja korábbi, mint egy perc. Körülbelül egy percet vesz igénybe a minták toopropagate hello rendszeren keresztül, így minták hello közé `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` nem érhető el. Ebben az esetben hello százalékos paraméterét használhatja `GetSample()` tooforce egy adott minta százalékos követelményt.

> [!IMPORTANT]
> A Microsoft **erősen ajánlott** , amikor **elkerülése érdekében a megbízható függő *csak* a `GetSample(1)` az automatikus skálázás képletekben**. Ennek az az oka `GetSample(1)` lényegében szerint toohello Batch szolgáltatás, a "Me hello rendelkezik, az utolsó minta adjon nem számít, hogy mennyivel beolvasni." Csak egy egyszeres, és lehet, hogy egy régebbi mintát, mert nem lehet képviselője hello nagyobb kép legutóbbi tevékenység vagy erőforrás-állapot. Ha a `GetSample(1)`, győződjön meg arról, hogy a nagyobb utasítás részét, és nem hello csak pontja, amely a képlet támaszkodik.
>
>

## <a name="metrics"></a>Mérőszámok
Erőforrás és a feladat metrikák képlet definiálásakor még használhatja. Hello készletben hello metrikák adatokon alapulnak beszerzése és értékelje ki dedikált csomópontok száma hello cél módosíthatja. Lásd: hello [változók](#variables) szakasz fölött mindegyik metrikát olvashat.

<table>
  <tr>
    <th>Metrika</th>
    <th>Leírás</th>
  </tr>
  <tr>
    <td><b>Erőforrás</b></td>
    <td><p>Erőforrás metrikák hello CPU, a hello sávszélesség, a számítási csomópontok hello memóriahasználata alapulnak, és hello csomópontok száma.</p>
        <p> A szolgáltatás által definiált változókat hasznosak korrekciók csomópontok száma alapján:</p>
    <p><ul>
            <li>$TargetDedicatedNodes</li>
            <li>$TargetLowPriorityNodes</li>
            <li>$CurrentDedicatedNodes</li>
            <li>$CurrentLowPriorityNodes</li>
            <li>$preemptedNodeCount</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>A szolgáltatás által definiált változókat hasznosak korrekciók csomópont erőforrás-használat alapján:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Tevékenység</b></td>
    <td><p>Feladat metrikák függőben, feladatok, például az aktív, hello állapotának alapulnak, és befejeződött. hello következő szolgáltatás által definiált változókat hasznos, ha a feladat mérőszámok alapján készletméretet korrekciók:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Az automatikus skálázás képlet írása
Kibővített kialakítást hozhat létre az automatikus skálázás képlet képező utasításokat, amelyek a fenti összetevők hello használja, majd egyesítése egy teljes képlet ezen utasításokat. Ebben a szakaszban egy példa automatikus skálázás képlet által végrehajtható műveleteket az egyes valós méretezési döntések tudjuk létrehozni.

Először is határozza meg az új automatikus skálázás képlet hello követelményei. hello képlet a következőket:

1. Növelje a készlet dedikált számítási csomópontok száma hello célja, ha a CPU-használata túl magas.
2. Egy dedikált számítási csomópontok száma hello cél csökkentéséhez, amikor a CPU-használat alacsony.
3. Mindig korlátozása hello too400 dedikált csomópontok maximális száma.

magas CPU-használat során csomópontok száma tooincrease hello hello utasításban, amely tölti fel a felhasználó által definiált változó határozza meg (`$totalDedicatedNodes`) 110 százalékát hello aktuális cél dedikált csomópontok száma, de csak akkor, ha értékkel hello minimális átlagos CPU-használat hello során az utolsó 10 percet 70 százalék felett volt. Ellenkező esetben használja a hello értéket a kijelölt csomópontok hello aktuális száma.

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
```

túl*csökkentése* hello dedikált csomópontok száma alacsony CPU-használat során, a képlet beállítása hello a következő utasítás azonos hello `$totalDedicatedNodes` hello aktuális cél dedikált csomópontok száma ha hello változó too90 százaléka átlagos CPU-használat hello elmúlt 60 perc volt a 20 százalékát. Ellenkező esetben használja az aktuális értéke hello `$totalDedicatedNodes` , amely jelenleg a fenti hello utasításban feltöltve.

```
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
```

Most korlátozása dedikált számítási csomópontok tooa legfeljebb 400 hello cél száma:

```
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

Az alábbiakban hello teljes képlet:

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

## <a name="create-an-autoscale-enabled-pool-with-net"></a>A .NET az automatikus skálázás engedélyezve készlet létrehozása

az automatikus skálázást engedélyezve van a .NET, a készlet toocreate kövesse az alábbi lépéseket:

1. Hozzon létre hello címkészlet, amely [BatchClient.PoolOperations.CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool).
2. Set hello [CloudPool.AutoScaleEnabled](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) tulajdonság túl`true`.
3. Set hello [CloudPool.AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) az automatikus skálázás képlettel tulajdonság.
4. (Választható) Set hello [CloudPool.AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) tulajdonság (alapértelmezett érték 15 perc).
5. Hello címkészlet, amely véglegesíti [CloudPool.Commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) vagy [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync).

hello következő kódrészletet létrehoz egy automatikus skálázás engedélyezve készlet .NET. hello készlet automatikus skálázás képlet beállítja hello cél számát dedikált csomópontok too5 hétfőn, és minden egyéb hello hét napján 1. Hello [automatikus méretezési időköz](#automatic-scaling-interval) van beállítva too30 perc. Ezen és egyéb C# kódtöredékek ebben a cikkben hello `myBatchClient` hello megfelelően inicializálva példánya [BatchClient] [ net_batchclient] osztály.

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool(
                    poolId: "mypool",
                    virtualMachineSize: "small", // single-core, 1.75 GB memory, 225 GB disk
                    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));    
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
await pool.CommitAsync();
```

> [!IMPORTANT]
> Az automatikus skálázás engedélyezve készletet hoz létre, ha nem ad meg hello _targetDedicatedComputeNodes_ paraméter vagy hello _targetLowPriorityComputeNodes_ hello paraméter hívja túl **CreatePool**. Ehelyett adja meg a hello **AutoScaleEnabled** és **AutoScaleFormula** hello készlet tulajdonságai. Ezen tulajdonságok hello értékek meghatározzák, hogy minden csomópont típusú hello cél száma. Emellett toomanually átméretezése az automatikus skálázás engedélyezve készlet (például a [BatchClient.PoolOperations.ResizePoolAsync][net_poolops_resizepoolasync]), első **tiltsa le a** az automatikus skálázás hello készletet, majd méretezze át.
>
>

Ezenkívül tooBatch .NET használhatja bármelyik más hello [kötegelt SDK-k](batch-apis-tools.md#azure-accounts-for-batch-development), [Batch REST](https://docs.microsoft.com/rest/api/batchservice/), [kötegelt PowerShell-parancsmagok](batch-powershell-cmdlets-get-started.md), és hello [kötegelt CLI](batch-cli-get-started.md)tooconfigure automatikus skálázást.


### <a name="automatic-scaling-interval"></a>Automatikus méretezési időköz
Alapértelmezés szerint hello Batch szolgáltatás beállítása szerint tooits automatikus skálázás képlet, 15 percenként a készlet méretét. Ez az időtartam alatt a következő alkalmazáskészlet tulajdonságaiban hello segítségével konfigurálható:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (kötegelt .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API-t)

hello minimális időköze az öt percet, és hello maximális 168 óra. Időközönkénti ezen a tartományon kívül van megadva, ha a Batch szolgáltatás hello egy hibás kérés (400) hibaüzenetet adja vissza.

> [!NOTE]
> Automatikus skálázás nem jelenleg tervezett toorespond toochanges kisebb, mint egy percen belül, de ahelyett, hogy a gyűjtő tooadjust hello mérete fokozatosan szánt futtatja, a munkaterhelés.
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a>Egy meglévő készlet automatikus skálázás engedélyezéséhez

Minden egyes kötegelt SDK biztosít egy módon tooenable automatikus skálázást. Példa:

* [BatchClient.PoolOperations.EnableAutoScaleAsync] [ net_enableautoscaleasync] (kötegelt .NET)
* [A készlet automatikus skálázás engedélyezése] [ rest_enableautoscale] (REST API-t)

Ha engedélyezi az automatikus skálázás meglévő címkészlet, tartsa szem előtt tartva hello pontok a következő:

* Ha az automatikus skálázás jelenleg le van tiltva a hello készlet hello kérelem tooenable automatikus skálázás küldésekor, adjon meg érvényes automatikus skálázás képlet hello kérés elküldésekor. Opcionálisan megadhat az automatikus skálázás újraértékelési időközét. Ha nem adja meg a időközt, hello alapértelmezett érték 15 perc szolgál.
* Ha az automatikus skálázás hello készlet jelenleg engedélyezve van, megadhatja az automatikus skálázás képlet, egy újraértékelési időközét, vagy mindkettőt. Ezek a tulajdonságok közül legalább egy meg kell adnia.

  * Ha egy új automatikus skálázás újraértékelési időközét adja meg, majd hello meglévő értékelésének ütemezése le van állítva, és új ütemezés szerint elindult. hello új ütemezés kezdési idő az hello idő, mely hello kérelem tooenable automatikus skálázás ki.
  * Kihagyása vagy hello automatikus skálázás képlet vagy a kiértékelési időköz, a Batch szolgáltatás hello toouse hello aktuális értéket továbbra is.

> [!NOTE]
> Ha a hello értéket adott meg *targetDedicatedComputeNodes* vagy *targetLowPriorityComputeNodes* hello paraméterei **CreatePool** metódus hello létrehozásakor a .NET, vagy egy másik nyelvet, majd ezeket az értékeket hello összehasonlítható paramétereinek alkalmazáskészlet figyelmen kívül lesznek hagyva, automatikus skálázás képlet hello kiértékelésekor.
>
>

A C# kódrészletet használja hello [Batch .NET] [ net_api] könyvtár tooenable automatikus skálázás meglévő címkészlet:

```csharp
// Define hello autoscaling formula. This formula sets hello target number of nodes
// too5 on Mondays, and 1 on every other day of hello week
string myAutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";

// Set hello autoscale formula on hello existing pool
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Az automatikus skálázás képlet frissítése

egy meglévő automatikus skálázás engedélyezve készlet, hívás hello művelet tooenable automatikus skálázás újra az új képlet hello tooupdate hello képletet. Például, ha az automatikus skálázás már engedélyezve van a `myexistingpool` eljárás végrehajtásakor a következő kód .NET hello, az automatikus skálázás képlet helyére hello tartalmát `myNewFormula`.

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-hello-autoscale-interval"></a>Frissítési hello automatikus skálázás időköz

tooupdate hello automatikus skálázás újraértékelési időközét meglévő automatikus skálázás engedélyezve-készlet, hívás hello művelet tooenable automatikus skálázás újra az új hello időköz. Például tooset hello automatikus skálázás értékelési időköz too60 perc már automatikus skálázás engedélyezve a .NET-készletre vonatkozó:

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Az automatikus skálázás képlet kiértékelése

A képlet kiértékelheti tooa készlet alkalmazása előtt. Ezzel a módszerrel tesztelheti hello képlet toosee hogyan kimutatásait kiértékelése előtt hello képlet üzembe helyezésre.

az automatikus skálázás képlet tooevaluate, először engedélyeznie kell egy érvényes képlettel hello készlet automatikus skálázást. egy készlet, amely még nem rendelkezik az automatikus skálázás képlet tootest engedélyezve van, hello egysoros képlet használata `$TargetDedicatedNodes = 0` Ha először engedélyezi az automatikus skálázást. Ezt követően használja a következő tooevaluate hello képletet tootest hello egyikét:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) vagy [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)

    A Batch .NET módszerekhez hello azonosító egy meglévő készlet és hello automatikus skálázás képlet tooevaluate tartalmazó karakterláncot.

* [Az automatikus méretezési képlet kiértékelése](https://docs.microsoft.com/rest/api/batchservice/evaluate-an-automatic-scaling-formula)

    A REST API-kérelem a adja meg a hello készlet hello URI-azonosító, és automatikus skálázási képletnek hello hello *autoScaleFormula* hello kérelemtörzset eleme. hello válasz hello művelet bármely kapcsolódó toohello képlet hiba információkat tartalmaz.

Ezen [Batch .NET] [ net_api] kódrészletet, azt az automatikus skálázás képlet kiértékeléséhez. Ha hello készlet nem rendelkezik automatikus skálázás engedélyezve van, engedélyezzük az első.

```csharp
// First obtain a reference tooan existing pool
CloudPool pool = await batchClient.PoolOperations.GetPoolAsync("myExistingPool");

// If autoscaling isn't already enabled on hello pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula tooenable autoscaling on the
    // pool. This formula is valid, but won't resize hello pool:
    await pool.EnableAutoScaleAsync(
        autoscaleFormula: "$TargetDedicatedNodes = {pool.CurrentDedicatedNodes};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScaleAsync calls tooonce every 30 seconds.
    // Because we want tooapply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here tooensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh hello properties of hello pool so that we've got the
    // latest value for AutoScaleEnabled
    await pool.RefreshAsync();
}

// We must ensure that autoscaling is enabled on hello pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // hello formula tooevaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform hello autoscale formula evaluation. Note that this code does not
    // actually apply hello formula toohello pool.
    AutoScaleRun eval =
        await batchClient.PoolOperations.EvaluateAutoScaleAsync(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print hello results of hello AutoScaleRun.
        // This will display hello values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply hello formula toohello pool since it evaluated successfully
        await batchClient.PoolOperations.EnableAutoScaleAsync(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output hello message associated with hello error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

A következő kódrészletet látható hello képlet sikeres értékelése eredménye hasonló:

```
AutoScaleRun.Results:
    $TargetDedicatedNodes=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Automatikus skálázás futtatása adatainak beolvasása

a képlet szerint végző tooensure várt, azt javasoljuk, hogy rendszeres időközönként ellenőrizze a kötegelt végzi, a készlet hello automatikus skálázás kísérletekről hello eredményeit. toodo tehát beolvasása (vagy frissítse) egy hivatkozás toohello tárolókészlet, és az utolsó automatikus skálázás futtatása hello tulajdonságainak vizsgálata.

A Batch .NET hello [CloudPool.AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) tulajdonság több tulajdonságai hello legújabb automatikus skálázás hello készlet elvégezni futtatnak ismertetik:

* [AutoScaleRun.Timestamp](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.timestamp)
* [AutoScaleRun.Results](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.results)
* [AutoScaleRun.Error](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.error)

Hello REST API-t, a hello [készlet adatainak beolvasása](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) kérelem hello készletbe, amely tartalmazza a hello legújabb automatikus skálázás hello információk futtatni információt ad vissza [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool#bk_autrun) tulajdonság.

hello következő C# kódrészletet használja hello Batch .NET könyvtár tooprint információ hello utolsó automatikus skálázás futtassa a készlet _myPool_:

```csharp
await Cloud pool = myBatchClient.PoolOperations.GetPoolAsync("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Részlet megelőző hello Példakimenet:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicatedNodes=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Példa automatikus skálázás képletek
Vizsgáljuk meg néhány képletek, amelyek különböző módokon tooadjust hello számítási erőforrások mennyisége a készletben.

### <a name="example-1-time-based-adjustment"></a>1. példa: Időalapú módosítása
Tegyük fel, hogy tooadjust hello készlet mérete alapján hello hét napja hello napját és idejét. Ez a példa bemutatja, hogyan tárolókészlet hello csomópontok tooincrease vagy csökken hello számának megfelelően.

hello képlet először beolvassa hello aktuális idő. Ha a hét napja (1-5) és munkaidőn belül (8 AM too6 PM), hello cél készletméretet too20 csomópontok van beállítva. Ellenkező esetben van állítva, akkor too10 csomópontok.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>2. példa: Feladatalapú módosítása
Ebben a példában hello készletméretet módosul hello várólistában lévő feladatok hello száma alapján. Megjegyzések és a sortöréseket a képlet karakterláncok elfogadhatók.

```csharp
// Get pending tasks for hello past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use hello last sample point,
// otherwise we use hello maximum of last sample point and hello history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM toopending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicatedNodes/2);
// hello pool size is capped at 20, if target VM value is more than that, set it
// too20. This value should be adjusted according tooyour use case.
$TargetDedicatedNodes = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>3. példa: Párhuzamos tevékenységek nyilvántartás
Ez a példa hello készletméretet hello feladatok száma alapján állítja be. A képlet is veszi figyelembe hello [MaxTasksPerComputeNode] [ net_maxtasks] hello készlethez beállított értéket. Ez a módszer akkor hasznos, ahol [párhuzamos tevékenység végrehajtása](batch-parallel-node-tasks.md) a készlet engedélyezve lett.

```csharp
// Determine whether 70 percent of hello samples have been recorded in hello past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set hello number of nodes tooadd tooone-fourth hello number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set too4, adjust this number
// for your use case)
$cores = $TargetDedicatedNodes * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicatedNodes + $extraVMs);
// Attempt toogrow hello number of compute nodes toomatch hello number of active
// tasks, with a maximum of 3
$TargetDedicatedNodes = max(0,min($targetVMs,3));
// Keep hello nodes active until hello tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>4. példa: Egy kezdeti mérete beállítása
Ez a példa azt mutatja be a C#-kód részlet az automatikus skálázás úgy, hogy beállítja a készlet méretének tooa hello csomópontok száma megadott egy kezdeti időszak. Majd helyesbíti hello készlet mérete alapján hello fut, és aktív feladatok után hello kezdeti időszak lejárt.

a következő kódrészletet hello hello képlet:

* Beállítja a hello kezdeti készlet mérete toofour csomópontok.
* Nem hello készlet méretének módosítása belül hello hello készlet életciklus első 10 perc.
* 10 perc elteltével beolvassa a hello maximális értékének hello fut a száma, és az aktív feladatok belül hello elmúlt 60 perc.
  * Ha mindkét értékek 0 (Ez azt jelzi, hogy egy feladat sem volt futó vagy hello aktív elmúlt 60 perc), hello készletméretet too0 van beállítva.
  * Ha bármelyik érték nullánál nem kisebb, nem történik változás.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicatedNodes = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicatedNodes = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicatedNodes) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Következő lépések
* [Azure Batch számítási erőforrás-használat csomópont egyidejű feladatok maximális](batch-parallel-node-tasks.md) részletesen ismerteti, hogyan hajthat végre több feladat egyidejű hello számítási csomópontok a készlethez. Továbbá tooautoscaling, ez a szolgáltatás segíthet bizonyos munkaterhelések esetén, így pénzt takaríthat toolower feladat időtartama.
* Egy másik hatékonyságát gyorsító győződjön meg arról, hogy a kötegelt kérelem lekérdezések hello hello legtöbb optimális módját a Batch szolgáltatás. Lásd: [hello Azure Batch szolgáltatás hatékony lekérdezéséhez](batch-efficient-list-queries.md) toolearn hogyan toolimit hello átmenő hello keresztülhaladnak a hálózaton, ha több ezer hello állapotának adatok mennyisége számítási csomópontok és a feladatok.

[net_api]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[net_batchclient]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient
[net_cloudpool_autoscaleformula]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula
[net_cloudpool_autoscaleevalinterval]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval
[net_enableautoscaleasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.enableautoscaleasync
[net_maxtasks]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.maxtaskspercomputenode
[net_poolops_resizepoolasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.resizepoolasync

[rest_api]: https://docs.microsoft.com/rest/api/batchservice/
[rest_autoscaleformula]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_autoscaleinterval]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_enableautoscale]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
