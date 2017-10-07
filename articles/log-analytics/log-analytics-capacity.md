---
title: "aaaCapacity és teljesítmény-megoldás az Azure Naplóelemzés |} Microsoft Docs"
description: "Használjon hello kapacitás és a teljesítmény megoldás Naplóelemzési toohelp megismerte a hello kapacitás, a Hyper-V-kiszolgálók."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 51617a6f-ffdd-4ed2-8b74-1257149ce3d4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.openlocfilehash: c47bb1e8bb9d4460b0241e89a616f3b356844b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="plan-hyper-v-virtual-machine-capacity-with-hello-capacity-and-performance-solution-preview"></a>Tervezze meg a Hyper-V virtuális gép kapacitásának hello kapacitást és teljesítményt megoldás (előzetes verzió)

![Kapacitást és teljesítményt szimbólum](./media/log-analytics-capacity/capacity-solution.png)

Hello kapacitás is használhatja, és ismernie Naplóelemzési toohelp teljesítmény megoldás hello kapacitás, a Hyper-V-kiszolgálók. hello megoldás azáltal, hogy bemutatja a Hyper-V környezetben betekintést hello teljes kihasználtság (Processzor, memória és lemez) hello állomások, és azokat Hyper-V gazdagépeken futó virtuális gépek hello biztosít. Metrikák Processzor, memória és a lemezek gyűjtése történt a gazdagépeken és a rajtuk futó hello virtuális gépek között.

hello megoldást:

-   A legkisebb és legnagyobb Processzor- és memóriafelhasználását gazdagépei láthatók
-   A legkisebb és legnagyobb Processzor- és memóriafelhasználását jeleníti meg a virtuális gépek
-   Virtuális gépek biztosítanak legnagyobb és legkisebb iops-érték és az átvitel mellett látható
-   Jeleníti meg, amely mely állomásokon futó virtuális gépek
-   Hello felső lemezek nagy átviteli sebességgel, IOPS, és a késés fürt megosztott kötetei jeleníti meg
- Lehetővé teszi az toocustomize és szűrő csoportok alapján

> [!NOTE]
> hello kapacitást és teljesítményt megoldást kínál kapacitás felügyeleti korábbi verziójának hello szükséges a System Center Operations Manager és a System Center Virtual Machine Manager. A frissített megoldás azok nem rendelkezik.


## <a name="connected-sources"></a>Összekapcsolt források

a következő táblázat hello hello csatlakoztatott adatforrások, ez a megoldás által támogatott ismerteti.

| Összekapcsolt forrás | Támogatás | Leírás |
|---|---|---|
| [Windows-ügynökök](log-analytics-windows-agents.md) | Igen | hello megoldás kapacitást és teljesítményt adatok információt gyűjt a Windows-ügynökök. |
| [Linux-ügynökök](log-analytics-linux-agents.md) | Nem    | hello megoldás nem kapacitást és teljesítményt adatok információkat gyűjtsön a közvetlen Linux-ügynököt.|
| [SCOM felügyeleti csoport](log-analytics-om-agents.md) | Igen |hello megoldás kapacitás és teljesítményadatokat gyűjt az ügynökök a csatlakoztatott SCOM felügyeleti csoport. Az SCOM-ügynököt tooOMS hello közvetlen kapcsolatra szükség. Adattárból hello felügyeleti csoport toohello OMS adat továbbítódik.|
| [Azure Storage-fiók](log-analytics-azure-storage.md) | Nem | Az Azure storage nem tartalmazza a kapacitást és teljesítményt adatait.|

## <a name="prerequisites"></a>Előfeltételek

- A Windows vagy az Operations Manager-ügynökök telepítenie kell a Windows Server 2012 vagy újabb Hyper-V gazdagépen, a nem a virtuális gépek.


## <a name="configuration"></a>Konfiguráció

Hajtsa végre a következő lépés tooadd hello kapacitást és teljesítményt megoldás tooyour munkaterület hello.

- Adja hozzá a kapacitás hello és teljesítmény megoldás tooyour OMS-munkaterület hello eljárással ismertetett [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).

## <a name="management-packs"></a>Felügyeleti csomagok

Ha az SCOM felügyeleti csoport csatlakoztatott tooyour OMS-munkaterület, majd a következő felügyeleti csomagok hello esetén telepítve lesz scom hozzá ehhez a megoldáshoz. Ezek a felügyeleti csomagok nem igényelnek további konfigurációs vagy karbantartási feladatokat.

- Microsoft.IntelligencePacks.CapacityPerformance

hello 1201 esemény hasonlít:


```
New Management Pack with id:"Microsoft.IntelligencePacks.CapacityPerformance", version:"1.10.3190.0" received.
```

Hello kapacitást és teljesítményt megoldás frissítésekor hello verziószám változik.

A megoldás felügyeleti csomagok frissítésének további információkért lásd: [csatlakozás az Operations Manager tooLog Analytics](log-analytics-om-agents.md).

## <a name="using-hello-solution"></a>Hello megoldással

Hello kapacitást és teljesítményt megoldás tooyour munkaterület hozzáadásakor hello kapacitást és teljesítményt kerül toohello áttekintő irányítópulthoz. Ez a csempe hello jelenleg aktív Hyper-V gazdagépek és hello száma volt figyelni a időszak kijelölt hello időpontig aktív virtuális gépek számát jeleníti meg.

![Kapacitást és teljesítményt csempe](./media/log-analytics-capacity/capacity-tile.png)


### <a name="review-utilization"></a>Tekintse át a kihasználtság

Kattintson a hello kapacitás és teljesítmény csempe tooopen hello kapacitást és teljesítményt irányítópultot. hello irányítópult szerepel a következő táblázat hello hello oszlopa. Minden oszlop megjeleníti, hogy az oszlop feltételek hello megadva hatókör és a kívánt időtartományt tooten elemet be. A napló keresési, amely visszaadja az összes rekord kattintva futtathatja **láthatja az összes** hello oszlop vagy hello oszlop fejlécére kattintva hello alján.

- **Gazdagépek**
    - **A gazdagép CPU-felhasználás** egy tendenciagrafikont hoz létre a számítógépek hello CPU-felhasználását és a gazdagépekhez, a kijelölt időszakban hello alapján listáját jeleníti meg. Vigye hello sor diagram tooview részletek az adott időben. Kattintson a további részleteket a naplófájl-keresési hello diagram tooview. Kattintson a bármely állomás neve tooopen napló keresés, és tekintse meg CPU teljesítményszámláló adatait üzemeltetett virtuális gépek esetén.
    - **Gazdagép memóriahasználata** egy tendenciagrafikont hoz létre hello memóriahasználata számítógépek és a gazdagépekhez, a kijelölt időszakban hello alapján listáját jeleníti meg. Vigye hello sor diagram tooview részletek az adott időben. Kattintson a további részleteket a naplófájl-keresési hello diagram tooview. Kattintson a bármely állomás neve tooopen napló keresési és memória teljesítményszámláló adatait üzemeltetett virtuális gépek esetén.
- **Virtuális gépek**
    - **VM CPU-felhasználás** egy tendenciagrafikont hoz létre a virtuális gépek hello CPU-kihasználtsági és a virtuális gépek, a kijelölt időszakban hello alapján listáját jeleníti meg. Vigye hello sor diagram tooview részletek az adott időben a hello felső 3 virtuális gép. Kattintson a további részleteket a naplófájl-keresési hello diagram tooview. Kattintson a virtuális gép neve tooopen napló keresési és hello VM összesített CPU számláló részleteinek megtekintése.
    - **Virtuális gép memória-felhasználás** egy tendenciagrafikont hoz létre a virtuális gépek memóriahasználata hello és virtuális gépek, a kijelölt időszakban hello alapján listáját jeleníti meg. Vigye hello sor diagram tooview részletek az adott időben a hello felső 3 virtuális gép. Kattintson a további részleteket a naplófájl-keresési hello diagram tooview. Kattintson a virtuális gép neve tooopen napló keresési és hello VM összesített memória számláló részleteinek megtekintése.
    - **Virtuális gép összes lemez IOPS** mutatja egy tendenciagrafikont hoz létre az hello teljes IOPS lemez a virtuális gépek és a virtuális gépek hello IOPS minden, a listája alapján hello kijelölt időszakban. Vigye hello sor diagram tooview részletek az adott időben a hello felső 3 virtuális gép. Kattintson a további részleteket a naplófájl-keresési hello diagram tooview. Kattintson a bármely virtuális gép neve tooopen napló keresési és összesített lemez IOPS számláló hello virtuális gép adatait.
    - **Virtuális gép teljes lemez átviteli sebesség** egy tendenciagrafikont hoz létre a virtuális gépek és a hello a lemez teljes átviteli az egyes virtuális gépek listájának, alapján hello hello a lemez teljes átviteli kijelölt időszakra vonatkozóan tartalmazza. Vigye hello sor diagram tooview részletek az adott időben a hello felső 3 virtuális gép. Kattintson a további részleteket a naplófájl-keresési hello diagram tooview. Kattintson a virtuális gép neve tooopen napló keresési és hello VM összesített a lemez teljes átviteli sebesség számláló részleteinek megtekintése.
- **A fürt megosztott kötetei**
    - **Teljes átviteli sebesség** hello összege is beolvassa, és a fürtözött megosztott köteteket jeleníti meg.
    - **Teljes IOPS** bemeneti/kimeneti műveletek másodpercenkénti száma a fürtözött megosztott kötetek hello összegét mutatja.
    - **Teljes késést** hello teljes késést jeleníti meg a fürt megosztott kötetei.
- **Gazdagép sűrűség** hello felső csempe hello gazdagépek és virtuális gépek rendelkezésre toohello megoldás teljes számát jeleníti meg. Kattintson a hello felső csempe tooview további részletek a naplóban keresési. Is felsorolja az összes gazdagép és virtuális gépek hello száma. Kattintson egy gazdagép toodrill be egy naplófájl-keresési hello VM eredményez.


![Irányítópult állomások panel](./media/log-analytics-capacity/dashboard-hosts.png)

![Irányítópult virtuális gépek panelje](./media/log-analytics-capacity/dashboard-vms.png)


### <a name="evaluate-performance"></a>Teljesítmény kiértékelése

Éles környezetekhez egy szervezet tooanother nagymértékben eltérnek. Emellett kapacitást és teljesítményt munkaterhelések függhet hogyan a virtuális gépek futnak, és Ön normál érdemes. Speciális eljárásokat toohelp mérni a teljesítmény akkor valószínűleg nem vonatkozik a tooyour környezetben. Így több épülő általánosítva van jobban olyan környezethez a legalkalmasabb toohelp. A Microsoft számos közzéteszi az előírásoknak megfelelő útmutató cikkek toohelp mérni a teljesítményt.

toosummarize, hello megoldás kapacitás és teljesítményadatokat gyűjt a különböző forrásokból, beleértve a teljesítményszámlálókat. Az, hogy a különböző felületek hello megoldásban bemutatott kapacitást és teljesítményt adatokat, és hasonlítsa össze a következő hello eredmények toothose [teljesítménye méri a Hyper-V](https://msdn.microsoft.com/library/cc768535.aspx) cikk. Bár a hello cikk régebben lett közzétéve, hello metrikákat, szempontok és irányelveket még érvényesek. hello cikk tooother hasznos források hivatkozásait tartalmazza.


## <a name="sample-log-searches"></a>Naplókeresési minták

a következő táblázat hello biztosít a kapacitást és teljesítményt adatokat gyűjt, és ez a megoldás által kiszámított minta napló keres.

| Lekérdezés | Leírás |
|---|---|
| Az összes állomás tárolómemória beállításai | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="Host Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Az összes virtuális gép memória konfigurációja | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="VM Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Minden virtuális gép közötti összes lemez IOPS bontása | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Reads/s" OR CounterName="VHD Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Minden virtuális gépek között a teljes lemez átviteli lebontása | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Read MB/s" OR CounterName="VHD Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Minden CSV-k között teljes IOPS bontása | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Reads/s" OR CounterName="CSV Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Teljes átviteli sebesség minden CSV-k között bontása | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read MB/s" OR CounterName="CSV Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Minden CSV-k között teljes késést bontása | <code> Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read Latency" OR CounterName="CSV Write Latency") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezések hello megváltozna toohello következő.

> | Lekérdezés | Leírás |
|:--- |:--- |
| Az összes állomás tárolómemória beállításai | A Teljesítményfigyelő &#124; Ha ObjectName == "Kapacitást és teljesítményt" és a CounterName == "Fogadó hozzárendelt memória (MB)" &#124; MB összefoglalója által példánynév avg(CounterValue) = |
| Az összes virtuális gép memória konfigurációja | A Teljesítményfigyelő &#124; Ha ObjectName == "Kapacitást és teljesítményt" és a CounterName == "Virtuális gép hozzárendelt memória (MB)" &#124; MB összefoglalója által példánynév avg(CounterValue) = |
| Minden virtuális gép közötti összes lemez IOPS bontása | A Teljesítményfigyelő &#124; Ha ObjectName == "Kapacitást és teljesítményt" és (CounterName == "VHD olvasási műveletek/mp" vagy a CounterName == "VHD írási műveletek/mp") &#124; AggregatedValue összefoglalója bin (TimeGenerated, 1 óra), amelyet avg(CounterValue) = CounterName, az InstanceName |
| Minden virtuális gépek között a teljes lemez átviteli lebontása | A Teljesítményfigyelő &#124; Ha ObjectName == "Kapacitást és teljesítményt" és (CounterName == "VHD olvasási MB/s" vagy a CounterName == "VHD írás MB/s") &#124; AggregatedValue összefoglalója bin (TimeGenerated, 1 óra), amelyet avg(CounterValue) = CounterName, az InstanceName |
| Minden CSV-k között teljes IOPS bontása | A Teljesítményfigyelő &#124; Ha ObjectName == "Kapacitást és teljesítményt" és (CounterName == "CSV olvasási műveletek/mp" vagy a CounterName == "CSV írási műveletek/mp") &#124; AggregatedValue összefoglalója bin (TimeGenerated, 1 óra), amelyet avg(CounterValue) = CounterName, az InstanceName |
| Teljes átviteli sebesség minden CSV-k között bontása | A Teljesítményfigyelő &#124; Ha ObjectName == "Kapacitást és teljesítményt" és (CounterName == "CSV olvasási műveletek/mp" vagy a CounterName == "CSV írási műveletek/mp") &#124; AggregatedValue összefoglalója bin (TimeGenerated, 1 óra), amelyet avg(CounterValue) = CounterName, az InstanceName |
| Minden CSV-k között teljes késést bontása | A Teljesítményfigyelő &#124; Ha ObjectName == "Kapacitást és teljesítményt" és (CounterName == "CSV olvasási késése" vagy a CounterName == "CSV írás késés") &#124; AggregatedValue összefoglalója bin (TimeGenerated, 1 óra), amelyet avg(CounterValue) = CounterName, az InstanceName |


## <a name="next-steps"></a>Következő lépések
* Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) tooview részletesebb kapacitását és teljesítményét.
