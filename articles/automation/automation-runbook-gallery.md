---
title: "az Azure Automation aaaRunbook és modul gyűjtemények |} Microsoft Docs"
description: "Forgatókönyveit és moduljait Microsoft és hello Közösségtől, tooinstall érhetők el, és az Azure Automation-környezetben.  Ez a cikk ismerteti, hogyan férhet hozzá ezen erőforrások és toocontribute a runbookok toohello gyűjteménye."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d3fee7b4-630a-4c10-8425-9bf51d7c9e58
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 10b634460edf66dd7548017e3a2f7111b7125f30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Az Azure Automation forgatókönyv- és minták
Ahelyett, hogy a saját forgatókönyveit és moduljait létrehozása az Azure Automationben, különféle forgatókönyvekhez, amik már a Microsoft és hello Közösség által készített végezheti el.  Használja a következő használati helyzetekben módosítás nélkül, vagy használhatja őket a kiindulási pontként, és szerkesztheti azokat az adott igények szerint.

A runbookok letölthető hello [forgatókönyvek](#runbooks-in-runbook-gallery) és hello modulok [PowerShell-galériában](#modules-in-powerShell-gallery).  Toohello közösségi is hozzájárulhat a most kialakított forgatókönyvek megosztásával.

## <a name="runbooks-in-runbook-gallery"></a>Runbookok a Runbook-galériában
Hello [forgatókönyvek](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) runbookok számos biztosít a Microsoft és hello Közösség, amely az Azure Automation importálhatja. Hello lévő hello gyűjteményből vagy letöltheti egy runbook [TechNet Script Center](https://gallery.technet.microsoft.com/scriptcenter/site/upload), akkor közvetlenül importálhatja runbookokat hello gyűjteményből hello a klasszikus Azure portálon vagy az Azure-portálon.

Csak importálhatja közvetlenül gyűjteményből hello Runbook hello a klasszikus Azure portálon vagy az Azure portál használatával. Ez a funkció a Windows PowerShell használatával nem hajtható végre.

> [!NOTE]
> Ellenőriznie kell a runbookokat hello forgatókönyvek beszerezni, és járjon el körültekintően telepítése, és futtassa azokat éles környezetben hello tartalmát. |}
> 
> 

### <a name="tooimport-a-runbook-from-hello-runbook-gallery-with-hello-azure-classic-portal"></a>a klasszikus Azure portálon hello forgatókönyvek hello runbookok tooimport
1. Hello Azure-portálon a gombra, **új**, **alkalmazásszolgáltatások**, **Automation**, **Runbook**, **a gyűjtemény**.
2. Válasszon egy kategóriát tooview runbookok kapcsolatos, és válassza ki a runbook tooview hozzá tartozó részletek. Ha azt szeretné, hello runbook választja, gombra hello jobbra mutató nyílra.
   
    ![Runbook-katalógus](media/automation-runbook-gallery/runbook-gallery.png)
3. Tekintse át a hello runbook hello tartalmát, és bármely követelményekre kell ügyelnie a hello leírás. Amikor elkészült, kattintson a hello jobbra mutató nyíl gombra.
4. Írja be a hello runbook adatait, majd hello pipa gombra. a program már kitölti hello runbook neve.
5. hello runbook megjelenik a hello **Runbookok** hello Automation-fiók lapján.

### <a name="tooimport-a-runbook-from-hello-runbook-gallery-with-hello-azure-portal"></a>tooimport hello forgatókönyvek hello Azure-portálon a runbookok
1. Hello Azure portál nyissa meg az Automation-fiók.
2. Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.
3. Kattintson a **Tallózás gyűjtemény** gombra.
   
    ![Keresse meg a gyűjtemény gomb](media/automation-runbook-gallery/browse-gallery-button.png)
4. Hello gyűjteményelem szeretne, és válassza ki azt a tooview keresse meg a részletes adatait.
   
    ![Keresse meg a gyűjtemény](media/automation-runbook-gallery/browse-gallery.png)
5. Kattintson a **nézet forráskódú projektként** tooview hello eleme hello [TechNet Script Center](http://gallery.technet.microsoft.com/).
6. tooimport egy elemet, kattintson rá tooview a részletes adatait, majd a hello **importálási** gombra.
   
    ![Importálás gomb](media/automation-runbook-gallery/gallery-item-detail.png)
7. Szükség esetén módosítsa hello runbook hello nevét, majd kattintson a **OK** tooimport hello runbook.
8. hello runbook megjelenik a hello **Runbookok** hello Automation-fiók lapján.

### <a name="adding-a-runbook-toohello-runbook-gallery"></a>Egy runbook toohello runbook gyűjteményelem hozzáadása
A Microsoft javasolja tooadd runbookok toohello Runbook gyűjteménye, amelyek úgy gondolja, hogy hasznos tooother ügyfelek lenne.  Hozzáadhat egy runbook által [ismét feltölteni a Script Center toohello](http://gallery.technet.microsoft.com/site/upload) figyelembe véve a következő adatok fiók hello.

* Meg kell adnia *Windows Azure* a hello **kategória** és *Automation* a hello **alkategóriát** hello runbook toobe jelenik meg a hello varázslóban.  
* hello feltöltés egyetlen .ps1 vagy .graphrunbook fájlnak kell lennie.  Ha hello runbook van szüksége, bármilyen modulok, gyermekforgatókönyvek vagy eszközök, majd sorolja fel azokat a hello küldésének hello leírása és a hello runbook hello megjegyzéseket tartalmazó részét.  Ha több runbook igénylő környezettel rendelkezik, majd töltse fel minden egyes külön-külön, és a lista hello nevei hello kapcsolódó runbookok minden azok leírásait tartalmazza. Győződjön meg arról, hogy használja-e azonos címkéket, hogy azok fog megjelenni hello hello ugyanilyen kategóriájú. A felhasználónak kell, hogy más forgatókönyvek állnak-e a szükséges hello forgatókönyv toowork tooread hello leírás tooknow.
* Adja hozzá a hello címke "GraphicalPS" közzétételekor a **grafikus forgatókönyvnek** (ez nem egy grafikus munkafolyamat). 
* A PowerShell vagy a PowerShell-munkafolyamati kódrészletet beszúrása hello leírás használatával **helyezze be a kódját a szakasz** ikonra.
* hello hello feltöltésének összegzés megjelenő hello forgatókönyvek találatok, részletes információkat, amelyek segítségére lesznek a felhasználók azonosítása hello funkció hello runbook kell biztosítania.
* A következő címkék toohello feltöltés hello egy toothree kell hozzárendelni.  hello runbook hello varázslóban a hello kategóriák, amelyek megfelelnek a címkék jelennek meg.  A listában nem szereplő címkéket hello varázsló figyelmen kívül. Ha nem adja meg a megfelelő címkéket, hello runbook lesz alatt hello feltüntetve más kategóriát.
  
  * Biztonsági mentés
  * Kapacitáskezelés
  * Változáskezelés
  * Megfelelőség
  * Fejlesztési / tesztelési környezetben
  * Vészhelyreállítás
  * Figyelés
  * Javítás
  * Kiépítés
  * Szervizkiszolgáló
  * Virtuális gép életciklusának kezelésére
* Automatizálási frissíti hello gyűjtemény óránként egyszer, így azonnal nem jelenik meg a hozzájárulások.

## <a name="modules-in-powershell-gallery"></a>A PowerShell-galériában modulok
PowerShell-modul a runbookokban használható parancsmagokat tartalmaznak, és az Azure Automationben telepítése meglévő modulok érhetők el hello [PowerShell-galériában](http://www.powershellgallery.com).  Indítsa el ezt a tárat a hello Azure-portálon, és közvetlenül az Azure Automation a telepítést, vagy letöltheti a fájlokat, és manuális telepítése.  Közvetlenül a klasszikus Azure portálon hello hello modulok nem telepíthető, de letöltheti azokat mint bármely más modulja a telepítést.

### <a name="tooimport-a-module-from-hello-automation-module-gallery-with-hello-azure-portal"></a>tooimport hello Azure-portálon az Automation-modul gyűjtemény hello egy modult
1. Hello Azure portál nyissa meg az Automation-fiók.
2. Kattintson a hello **eszközök** csempe tooopen hello eszközök listája.
3. Kattintson a hello **modulok** csempe tooopen hello listájához.
4. Kattintson a hello **Tallózás gyűjtemény** gombra és hello Tallózás gyűjtemény panel nincs elindítva.
   
    ![A modul gyűjteménye](media/automation-runbook-gallery/modules-blade.png) <br>
5. Hello Tallózás gyűjtemény panel indítása, miután úgy keresheti meg a következő mezők hello:
   
   * A modul neve
   * Címkék
   * Szerző
   * A parancsmag/DSC-erőforrás neve
6. A modul is érdekli, és válassza ki azt a tooview keresse meg a részletes adatait.  
   Amikor egy adott modult elemezze, megtekintheti a hello modul, beleértve a hivatkozás hátsó toohello további információ a PowerShell-galériában el a szükséges függőségek, és minden hello parancsmagok és/vagy modul hello DSC erőforrásokat tartalmaz.
   
    ![PowerShell modul részletei](media/automation-runbook-gallery/gallery-item-details-blade.png) <br>
7. tooinstall hello modul közvetlenül az Azure Automation szolgáltatásbeli kattintson hello **importálási** gombra.
   
    ![Importálás modul gombra](media/automation-runbook-gallery/module-import-button.png)
8. Hello Importálás gombra kattintva, látni fogja, hogy a program tooimport kapcsolatos hello modulnév. Ha az összes hello függősége telepítve van, hello **OK** gomb aktív lesz. Ha függőségek hiányzik, akkor tooimport azokat, ez a modul importálása előtt.
9. Kattintson a **OK** tooimport hello modul, és hello modul panel elindul. Azure Automation importálására a modul tooyour fiókot, ha hello modul és hello parancsmagok metaadatainak bontja ki.
   
    ![Importálás modul panel](media/automation-runbook-gallery/module-import-blade.png)
   
    Ez eltarthat néhány percig, mivel az egyes tevékenységek kibontása toobe kell.
10. Egy értesítést fog kapni, hogy hello modul telepítése és egy értesítést, amikor befejeződött.
11. Hello modul az importálása után hello elérhető tevékenységeket látni fogja, és a runbookok és a célállapot-konfiguráció erőforrásait is használhatja.

## <a name="requesting-a-runbook-or-module"></a>A kért egy runbook vagy modul
Túl küldhet a kérelmek[User Voice](https://feedback.azure.com/forums/246290-azure-automation/).  Ha kell súgó írása egy runbookot, vagy PowerShell kapcsolatos kérdése van, írjon egy kérdés tooour [fórum](http://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Következő lépések
* a runbookok, használatába tooget lásd: [létrehozása vagy egy Azure Automation forgatókönyv importálása](automation-creating-importing-runbook.md)
* toounderstand hello közötti különbségek PowerShell és a PowerShell-munkafolyamati runbookokkal, lásd: [tanulási PowerShell munkafolyamat](automation-powershell-workflow.md)

