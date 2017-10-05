---
title: "Azure Automation-Runbook típusok |} Microsoft Docs"
description: "Ismerteti a runbookok, amelyek az Azure Automation és kapcsolatos szempontokat, akkor figyelembe kell vennie annak meghatározása, amelyek használatához írja be. "
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9265c975-4281-4819-a84f-d86641277f36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/01/2017
ms.author: bwren
ms.openlocfilehash: e859aef473b433fbf4efb639962f3a3ce0a23d7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-runbook-types"></a>Azure Automation-runbook típusok
Azure Automation szolgáltatásbeli, amely röviden ismerteti a runbookok négy típusokat támogatja az alábbi táblázatban.  Az alábbi szakaszokban minden típusát, melynek során vegye figyelembe az egyes esetekben a további információt.

| Típus | Leírás |
|:--- |:--- |
| [Grafikus](#graphical-runbooks) |A Windows PowerShell és az Azure-portálon létrehozott és szerkesztett teljesen a grafikus szerkesztő alapján. |
| [Grafikus PowerShell-munkafolyamat](#graphical-runbooks) |A Windows PowerShell munkafolyamat alapú és létrehozott és szerkesztett teljesen a grafikus szerkesztőben Azure-portálon. |
| [PowerShell](#powershell-runbooks) |A Windows PowerShell-parancsfájl alapján szöveg runbook. |
| [PowerShell-munkafolyamat](#powershell-workflow-runbooks) |A Windows PowerShell munkafolyamat alapú szöveg runbook. |

## <a name="graphical-runbooks"></a>Grafikus forgatókönyvek
[Grafikus](automation-runbook-types.md#graphical-runbooks) és grafikus PowerShell-munkafolyamati forgatókönyvek létrehozott és szerkesztett a grafikus szerkesztővel az Azure portálon.  Egy fájlba exportálhatja, és majd importálja azokat egy másik automation-fiók, de nem készít vagy szerkeszt őket egy másik eszközzel.  Grafikus forgatókönyvek létrehozása a PowerShell-kódot, de közvetlenül nem tekinthetők meg és módosíthatja a kódot. Grafikus forgatókönyvek nem alakítható át egyik a [szöveges formátumokból](automation-runbook-types.md), sem szöveg runbook konvertálható grafikus formátumban. Grafikus forgatókönyvek konvertálható grafikus PowerShell-munkafolyamati forgatókönyvek során importálása és fordítva.

### <a name="advantages"></a>Előnyei
* Visual szerzői modell insert-kapcsolat konfigurálása  
* Hogyan adatáramlás a folyamatot, összpontosítani  
* Vizuálisan képviselik a felügyeleti folyamatok  
* Közé tartozik más runbookok gyermek runbookként magas szintű munkafolyamatok létrehozásához  
* Könnyen létrejöhetnek moduláris programozás  


### <a name="limitations"></a>Korlátozások
* Nem lehet szerkeszteni a runbookot Azure-portálon kívül.
* A PowerShell-kódjába komplex logikai végrehajtásához tartalmazó tevékenységeket lehet szükség.
* Nem tekinthetők meg és a PowerShell-kódot, amely a grafikus munkafolyamat által létrehozott közvetlen szerkesztése. Vegye figyelembe, hogy látható kód tevékenységeket hoz létre a kódot.

## <a name="powershell-runbooks"></a>A PowerShell-forgatókönyvek
A PowerShell-forgatókönyvek a Windows PowerShell alapulnak.  Közvetlenül szerkesztheti a kódot a runbook a szövegszerkesztő segítségével az Azure portálon.  Is használhatja a kapcsolat nélküli szövegszerkesztőben és [a forgatókönyv importálása](http://msdn.microsoft.com/library/azure/dn643637.aspx) az Azure Automation.

### <a name="advantages"></a>Előnyei
* PowerShell-kód nélkül a további összetett szolgáltatásokkal, PowerShell-munkafolyamat minden komplex logikai megvalósításához. 
* Runbook gyorsabb, mint a PowerShell-munkafolyamati forgatókönyvek kezdődik, mivel fordítandó futtatása előtt nem kell azt.

### <a name="limitations"></a>Korlátozások
* PowerShell parancsfájl-kezelési ismernie kell.
* Nem használható [párhuzamos feldolgozást végző](automation-powershell-workflow.md#parallel-processing) párhuzamosan több műveletek elvégzéséhez.
* Nem használható [ellenőrzőpontokat](automation-powershell-workflow.md#checkpoints) folytatja a runbook hiba esetén.
* PowerShell-munkafolyamati forgatókönyvek és grafikus forgatókönyvek csak szerepelhetnek gyermek runbookként a Start-AzureAutomationRunbook parancsmaggal létrehoz egy új feladatot.

### <a name="known-issues"></a>Ismert problémák
Az alábbiakban a PowerShell-forgatókönyvek jelenlegi ismert problémái.

* A PowerShell-forgatókönyvek nem nem olvasható be egy titkosítatlan [változóeszköz](automation-variables.md) null értékű.
* A PowerShell-forgatókönyvek nem tud beolvasni egy [változóeszköz](automation-variables.md) a  *~*  a neve.
* Get-Process egy PowerShell ismétlődő runbook körülbelül 80 közelítő összeomolhat. 
* A PowerShell-forgatókönyv sikertelen lehet, ha nagyon nagy mennyiségű adatot írni a kimeneti adatfolyamba egyszerre megkísérli.   Általában oldható meg a probléma által szerint kiírta volna csak a nagyméretű objektumok az használatakor szükséges információkat.  Például helyett szerint kiírta volna hasonlót *Get-Process*, csak a kötelező mezőket a kimenetre küldheti *Get-Process |} Válassza ki a Folyamatnév, CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShell-munkafolyamati forgatókönyvek
PowerShell-munkafolyamati forgatókönyvek alapján szöveg runbookok olyan [Windows PowerShell munkafolyamat](automation-powershell-workflow.md).  Közvetlenül szerkesztheti a kódot a runbook a szövegszerkesztő segítségével az Azure portálon.  Is használhatja a kapcsolat nélküli szövegszerkesztőben és [a forgatókönyv importálása](http://msdn.microsoft.com/library/azure/dn643637.aspx) az Azure Automation.

### <a name="advantages"></a>Előnyei
* PowerShell-munkafolyamati kód minden komplex logikai megvalósításához.
* Használjon [ellenőrzőpontokat](automation-powershell-workflow.md#checkpoints) folytatja a runbook hiba esetén.
* Használjon [párhuzamos feldolgozást végző](automation-powershell-workflow.md#parallel-processing) párhuzamosan több műveletek elvégzéséhez.
* Grafikus forgatókönyvek és lehetnek PowerShell munkafolyamat runbookok gyermek runbookként magas szintű munkafolyamatok létrehozásához.

### <a name="limitations"></a>Korlátozások
* PowerShell-munkafolyamati ismernie kell a szerző.
* A Runbook például a PowerShell-munkafolyamati további összetettsége kell foglalkozik [objektumok deszerializálni](automation-powershell-workflow.md#code-changes).
* A Runbook elindításához mint a PowerShell-forgatókönyvek, mivel futtatása előtt kell lefordítani hosszabb időt vesz igénybe.
* A PowerShell-forgatókönyvek csak szerepelhetnek gyermek runbookként a Start-AzureAutomationRunbook parancsmaggal létrehoz egy új feladatot.

## <a name="considerations"></a>Megfontolandó szempontok
Akkor figyelembe kell vennie a következő további szempontok számának meghatározásakor az egyes runbookok használandó típust.

* A grafikus forgatókönyvek nem konvertálható szöveges típusra, vagy fordítva.
* Különböző típusú runbookok használja, a gyermek runbook korlátozások is.  Lásd: [az Azure Automation runbookjai gyermek](automation-child-runbooks.md) további információt.

## <a name="next-steps"></a>Következő lépések
* További információk a létrehozásról grafikus forgatókönyvnek, [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md)
* PowerShell és a PowerShell közötti különbségek megismeréséhez munkafolyamatok runbookok, lásd: [tanulási Windows PowerShell munkafolyamat](automation-powershell-workflow.md)
* Hozzon létre vagy Runbook importálása további információkért lásd: [létrehozása vagy egy Runbook importálása](automation-creating-importing-runbook.md)

