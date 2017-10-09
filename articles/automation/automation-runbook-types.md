---
title: "Automation-Runbook típusok aaaAzure |} Microsoft Docs"
description: "Bemutatja a hello különféle runbookokat, használhatja az Azure Automation és kapcsolatos szempontokat, akkor figyelembe kell vennie milyen típusú toouse meghatározásakor. "
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
ms.openlocfilehash: c28aa57c77025764b16784372308a4ff2f596914
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-runbook-types"></a>Azure Automation-runbook típusok
Azure Automation hello a következő táblázat röviden ismerteti a runbookok négy típusú támogatja.  hello az alábbi szakaszok nyújtanak további információt a mikor szempontokat is minden toouse.

| Típus | Leírás |
|:--- |:--- |
| [Grafikus](#graphical-runbooks) |A Windows PowerShell és az Azure-portálon létrehozott és szerkesztett teljesen a grafikus szerkesztő alapján. |
| [Grafikus PowerShell-munkafolyamat](#graphical-runbooks) |A Windows PowerShell munkafolyamat és a létrehozott és szerkesztett teljesen hello grafikus szerkesztő Azure-portálon alapján. |
| [PowerShell](#powershell-runbooks) |A Windows PowerShell-parancsfájl alapján szöveg runbook. |
| [PowerShell-munkafolyamat](#powershell-workflow-runbooks) |A Windows PowerShell munkafolyamat alapú szöveg runbook. |

## <a name="graphical-runbooks"></a>Grafikus forgatókönyvek
[Grafikus](automation-runbook-types.md#graphical-runbooks) és grafikus PowerShell-munkafolyamati forgatókönyvek létrehozott és szerkesztett hello grafikus szerkesztővel hello Azure-portálon a.  Tooa-fájl exportálását, és majd importálja azokat egy másik automation-fiók, de nem készít vagy szerkeszt őket egy másik eszközzel.  Grafikus forgatókönyvek létrehozása a PowerShell-kódot, de közvetlenül nem tekinthetők meg és hello kód módosítása. Grafikus forgatókönyvek nem lehet a hello konvertált tooone [szöveges formátumokból](automation-runbook-types.md), és nem lehet szöveg runbook konvertált toographical formátumban. Grafikus forgatókönyvek lehet konvertált tooGraphical PowerShell munkafolyamat-forgatókönyvekről során importálása és fordítva.

### <a name="advantages"></a>Előnyei
* Visual szerzői modell insert-kapcsolat konfigurálása  
* Hogyan adatáramlás hello folyamat összpontosítani  
* Vizuálisan képviselik a felügyeleti folyamatok  
* Egyéb forgatókönyvek közé gyermek runbookok toocreate magas szintű munkafolyamatok  
* Könnyen létrejöhetnek moduláris programozás  


### <a name="limitations"></a>Korlátozások
* Nem lehet szerkeszteni a runbookot Azure-portálon kívül.
* A PowerShell kód tooperform összetett logikát tartalmazó tevékenységeket lehet szükség.
* Nem tekinthetők meg és közvetlen szerkesztése hello grafikus munkafolyamat által létrehozott hello PowerShell-kódot. Vegye figyelembe, hogy látható-hello kód kód tevékenységeket hoz létre.

## <a name="powershell-runbooks"></a>A PowerShell-forgatókönyvek
A PowerShell-forgatókönyvek a Windows PowerShell alapulnak.  Közvetlenül szerkesztheti hello kód hello runbook hello szövegszerkesztőben hello Azure-portál használatával.  Is használhatja a kapcsolat nélküli szövegszerkesztőben és [hello forgatókönyv importálása](http://msdn.microsoft.com/library/azure/dn643637.aspx) az Azure Automation.

### <a name="advantages"></a>Előnyei
* PowerShell-kód nélkül hello további bonyolultságára PowerShell-munkafolyamat minden komplex logikai megvalósításához. 
* Runbook gyorsabb, mint a PowerShell-munkafolyamati forgatókönyvek kezdődik, mivel toobe összeállított futtatása előtt nem szükséges.

### <a name="limitations"></a>Korlátozások
* PowerShell parancsfájl-kezelési ismernie kell.
* Nem használható [párhuzamos feldolgozást végző](automation-powershell-workflow.md#parallel-processing) tooperform párhuzamosan több művelet.
* Nem használható [ellenőrzőpontokat](automation-powershell-workflow.md#checkpoints) tooresume runbook hiba esetén.
* PowerShell-munkafolyamati forgatókönyvek és grafikus forgatókönyvek csak szerepelhetnek gyermek runbookként hello Start-AzureAutomationRunbook parancsmaggal, amely létrehoz egy új feladatot.

### <a name="known-issues"></a>Ismert problémák
Az alábbiakban a PowerShell-forgatókönyvek jelenlegi ismert problémái.

* A PowerShell-forgatókönyvek nem nem olvasható be egy titkosítatlan [változóeszköz](automation-variables.md) null értékű.
* A PowerShell-forgatókönyvek nem tud beolvasni egy [változóeszköz](automation-variables.md) a  *~*  hello nevében.
* Get-Process egy PowerShell ismétlődő runbook körülbelül 80 közelítő összeomolhat. 
* A PowerShell-forgatókönyv sikertelen lehet, ha egyszerre megkísérel toowrite nagyon nagy mennyiségű adatok toohello kimeneti adatfolyam.   Általában oldható meg a probléma által írását, csak ha nagyméretű objektumok használata hello információkat.  Például helyett szerint kiírta volna hasonlót *Get-Process*, csak szükséges hello mezőket a kimenetre küldheti *Get-Process |} Válassza ki a Folyamatnév, CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShell-munkafolyamati forgatókönyvek
PowerShell-munkafolyamati forgatókönyvek alapján szöveg runbookok olyan [Windows PowerShell munkafolyamat](automation-powershell-workflow.md).  Közvetlenül szerkesztheti hello kód hello runbook hello szövegszerkesztőben hello Azure-portál használatával.  Is használhatja a kapcsolat nélküli szövegszerkesztőben és [hello forgatókönyv importálása](http://msdn.microsoft.com/library/azure/dn643637.aspx) az Azure Automation.

### <a name="advantages"></a>Előnyei
* PowerShell-munkafolyamati kód minden komplex logikai megvalósításához.
* Használjon [ellenőrzőpontokat](automation-powershell-workflow.md#checkpoints) tooresume runbook hiba esetén.
* Használjon [párhuzamos feldolgozást végző](automation-powershell-workflow.md#parallel-processing) tooperform párhuzamosan több művelet.
* Grafikus forgatókönyvek és lehetnek PowerShell munkafolyamat-forgatókönyvekről munkafolyamatként gyermek runbookok toocreate magas szintű.

### <a name="limitations"></a>Korlátozások
* PowerShell-munkafolyamati ismernie kell a szerző.
* A Runbook például a PowerShell-munkafolyamat hello nagyobb fokú összetettségével kell foglalkozik [objektumok deszerializálni](automation-powershell-workflow.md#code-changes).
* Runbook tart az hosszabb, mint a PowerShell-forgatókönyvek toostart óta toobe futtatása előtt össze kell.
* A PowerShell-forgatókönyvek csak szerepelhetnek gyermek runbookként hello Start-AzureAutomationRunbook parancsmaggal, amely létrehoz egy új feladatot.

## <a name="considerations"></a>Megfontolandó szempontok
Fiók hello milyen típusú toouse az egyes runbookok számának meghatározásakor a következő további szempontokat kell figyelembe.

* A runbookok grafikus tootextual típusból vagy viszont nem konvertálható.
* Különböző típusú runbookok használja, a gyermek runbook korlátozások is.  Lásd: [az Azure Automation runbookjai gyermek](automation-child-runbooks.md) további információt.

## <a name="next-steps"></a>Következő lépések
* toolearn grafikus forgatókönyvnek szerzői, kapcsolatos további információkért lásd: [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md)
* PowerShell és a PowerShell közötti különbségek toounderstand hello runbookok munkafolyamatok című [tanulási Windows PowerShell munkafolyamat](automation-powershell-workflow.md)
* További információ a hogyan toocreate vagy egy Runbook importálása: [létrehozása vagy egy Runbook importálása](automation-creating-importing-runbook.md)

