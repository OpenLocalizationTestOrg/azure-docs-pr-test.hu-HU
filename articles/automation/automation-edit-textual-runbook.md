---
title: "aaaEditing szöveges Azure Automation runbookjai"
description: "Ez a cikk biztosít a különböző eljárásokkal a PowerShell és a PowerShell-munkafolyamati forgatókönyvek az Azure Automationben használata hello szöveges szerkesztőjével."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: 6f5b48fb-6f30-4e99-9e14-9061b5554b08
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 3fd87d457838f300ca6c94bc345e82c679a0e011
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Azure Automation runbookjai szöveges szerkesztése
hello az Azure Automationben szöveges szerkesztő lehet használt tooedit [PowerShell-forgatókönyvek](automation-runbook-types.md#powershell-runbooks) és [PowerShell munkafolyamat-forgatókönyvekről](automation-runbook-types.md#powershell-workflow-runbooks). Azt hello tipikus részeit, más kód szerkesztőt, például az intellisense és kódolása további speciális funkciók tooassist rendelkező, a közös toorunbooks erőforrások elérése színét.  Ez a cikk nyújt a részletes lépéseket a szerkesztővel különböző funkcióihoz.

hello szöveges szerkesztő runbookba tevékenységeket, eszközök és a gyermekrunbookok szolgáltatás tooinsert kódot tartalmaz. Ahelyett, hogy írja be a kódját hello saját magának, a rendelkezésre álló erőforrások listájából válassza ki, és hello hello runbook beszúrt megfelelő kódot.

Azure Automation összes runbookjának két verziója van, vázlat és egy közzétett. Hello runbook vázlatverzióját hello szerkesztheti, és majd közzétenni ahhoz végrehajtható legyen. hello közzétett verzió nem szerkeszthető. Lásd: [runbook közzététele](automation-creating-importing-runbook.md#publishing-a-runbook) további információt.

a toowork [grafikus forgatókönyvek](automation-runbook-types.md#graphical-runbooks), lásd: [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md).

## <a name="tooedit-a-runbook-with-hello-azure-portal"></a>tooedit runbookkal a hello Azure-portálon
A következő eljárás tooopen szerkesztését hello szöveges szerkesztő a runbook hello használata.

1. Hello Azure-portálon válassza ki az automation-fiók.
2. Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.
3. Kattintson a hello neve hello runbook tooedit szeretne, és kattintson a hello **szerkesztése** gombra.
4. Hajtsa végre a szükséges szerkesztési hello.
5. Kattintson a **mentése** amikor a módosítások nem fejeződik.
6. Kattintson a **közzététel** Ha azt szeretné, hogy hello legfrissebb vázlatverzióját hello runbook toobe közzé.

### <a name="tooinsert-a-cmdlet-into-a-runbook"></a>tooinsert át egy runbooknak parancsmag
1. A vásznon a hello szöveges szerkesztő hello vigye hello kurzor tooplace hello parancsmag.
2. Bontsa ki a hello **parancsmagok** hello könyvtár vezérlő csomópontja.
3. Bontsa ki a kívánt toouse hello parancsmag tartalmazó hello modul.
4. Hello parancsmag tooinsert kattintson jobb gombbal, majd válassza ki **toocanvas hozzáadása**.  Ha hello parancsmag beállítása egynél több paraméterrel rendelkezik, majd hello alapértelmezés szerint megjelenik.  Hello parancsmag tooselect eltérő kibontva beállítása.
5. a paraméterek teljes listája a hello parancsmag hello kódját egészül ki.
6. Adjon meg egy megfelelő értéket, a szükséges paramétereket a kapcsos zárójelek <> hello adattípus helyett.  Távolítsa el a felesleges paramétereket.

### <a name="tooinsert-code-for-a-child-runbook-into-a-runbook"></a>a gyermek runbook át egy runbooknak tooinsert kódja
1. A vásznon a hello szöveges szerkesztő hello, vigye hello kurzor tooplace hello kód a hello [gyermekrunbook](automation-child-runbooks.md).
2. Bontsa ki a hello **Runbookok** hello könyvtár vezérlő csomópontja.
3. Hello runbook tooinsert kattintson jobb gombbal, majd válassza ki **toocanvas hozzáadása**.
4. a runbook paramétereket bármely helyőrzőkkel hello gyermekrunbook hello kódját egészül ki.
5. Hello helyőrzőket cserélje le minden paraméterhez megfelelő értéket.

### <a name="tooinsert-an-asset-into-a-runbook"></a>egy eszköz át egy runbooknak tooinsert
1. A vásznon a hello szöveges szerkesztő hello vigye hello kurzor tooplace hello kód hello gyermek runbook.
2. Bontsa ki a hello **eszközök** hello könyvtár vezérlő csomópontja.
3. Bontsa ki a kívánt eszköz hello típusú hello csomópontot.
4. Hello eszköz tooinsert kattintson jobb gombbal, majd válassza ki **toocanvas hozzáadása**.  A [változó eszközök](automation-variables.md), válassza **hozzáadása "Változó beolvasása" toocanvas** vagy **hozzáadása "Változó beállítása" toocanvas** attól függően, hogy szeretné, hogy tooget vagy hello változót.
5. hello kód hello eszköz hello runbook bekerülnek.

## <a name="tooedit-a-runbook-with-hello-azure-portal"></a>tooedit runbookkal a hello Azure-portálon
A következő eljárás tooopen szerkesztését hello szöveges szerkesztő a runbook hello használata.

1. Hello Azure-portálon, válassza ki **Automation** és majd kattintson az automation-fiók hello nevére.
2. Jelölje be hello **Runbookok** fülre.
3. Kattintson a hello neve hello runbook tooedit szeretne, és válassza a hello **Szerző** fülre.
4. Kattintson a hello **szerkesztése** üdvözlő képernyőt hello alján gombra.
5. Hajtsa végre a szükséges szerkesztési hello.
6. Kattintson a **mentése** amikor a módosítások nem fejeződik.
7. Kattintson a **közzététel** Ha azt szeretné, hogy hello legfrissebb vázlatverzióját hello runbook toobe közzé.

### <a name="tooinsert-an-activity-into-a-runbook"></a>egy Runbookban egy tevékenységet tooinsert
1. A vásznon a hello szöveges szerkesztő hello vigye hello kurzor tooplace hello tevékenység.
2. Hello a hello képernyő aljára, kattintson a **beszúrása** , majd **tevékenység**.
3. A hello **integrációs modul** oszlop, jelölje be hello modul, amely a hello tevékenységet tartalmaz.
4. A hello **tevékenység** ablaktáblán válasszon ki egy tevékenységet.
5. A hello **leírás** oszlop, hello tevékenység Megjegyzés hello leírását. Másik lehetőségként kattinthat a részletes nézet toolaunch súgó hello tevékenység hello böngészőben.
6. Kattintson a hello jobbra mutató nyílra.  Ha hello tevékenység paraméterekkel rendelkezik, azok az információt megjelenik.
7. Hello ellenőrzése gombra.  Hello runbook toorun hello tevékenységeket lesz.
8. Ha a hello tevékenységhez paraméterek szükségesek, adjon meg egy megfelelő értéket a csúcsos zárójelek <> között hello adattípus helyett.

### <a name="tooinsert-code-for-a-child-runbook-into-a-runbook"></a>a gyermek runbook át egy runbooknak tooinsert kódja
1. A vásznon a hello szöveges szerkesztő hello, vigye hello kurzor tooplace hello [gyermekrunbook](automation-child-runbooks.md).
2. Hello a hello képernyő aljára, kattintson a **beszúrása** , majd **Runbook**.
3. Hello runbook tooinsert hello középső oszlopból válassza ki, és hello jobbra mutató nyílra.
4. Ha hello runbook paraméterekkel rendelkezik, azok az információt megjelenik.
5. Hello ellenőrzése gombra.  Kód toorun hello kiválasztott runbook program beszúrja hello aktuális runbookot.
6. Ha a hello runbook paramétereket igényel, adjon meg egy megfelelő értéket a csúcsos zárójelek <> között hello adattípus helyett.

### <a name="tooinsert-an-asset-into-a-runbook"></a>egy eszköz át egy runbooknak tooinsert
1. A vásznon a hello szöveges szerkesztő hello vigye hello kurzor tooplace hello tevékenység tooretrieve hello eszköz.
2. Hello a hello képernyő aljára, kattintson a **beszúrása** , majd **beállítás**.
3. A hello **beállítási művelet** oszlop, válassza hello kívánt műveletet.
4. Válassza ki a hello hello középső oszlopban elérhető eszközök.
5. Hello ellenőrzése gombra.  Kód tooget vagy set hello eszköz program beszúrja a hello runbook.

## <a name="tooedit-an-azure-automation-runbook-using-windows-powershell"></a>tooedit egy Azure Automation-runbook Windows PowerShell használatával
egy runbook tooedit a Windows PowerShell használatával, az Ön által választott hello szerkesztővel, és mentse tooa .ps1 fájlt. Használhatja a hello [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) parancsmag tooretrieve hello hello runbook tartalmát, majd [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) parancsmag tooreplace hello meglévő runbook vázlatként megjelölt hello módosítani egy.

### <a name="tooretrieve-hello-contents-of-a-runbook-using-windows-powershell"></a>tooRetrieve hello tartalmát egy Runbook Windows PowerShell használatával
hello a következő mintaparancsok bemutatják, hogyan tooretrieve hello egy runbook parancsfájlja, és mentse tooa parancsfájl. Ebben a példában a rendszer lekéri hello vázlatként megjelölt verziót. Célszerű is lehetséges tooretrieve hello közzétett verzió hello runbook Bár jelen verziója nem módosítható.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="toochange-hello-contents-of-a-runbook-using-windows-powershell"></a>tooChange hello tartalmát egy Runbook Windows PowerShell használatával
hello következő mintaparancsok bemutatják, hogyan tooreplace hello hello tartalma a parancsfájl egy runbook meglévő tartalma. Vegye figyelembe, hogy ez az van hello azonos eljárást, mint a minta [tooimport egy runbookot, a Windows PowerShell-lel parancsfájlból](automation-creating-importing-runbook.md).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Létrehozása vagy egy Azure Automation forgatókönyv importálása](automation-creating-importing-runbook.md)
* [Learning PowerShell munkafolyamat](automation-powershell-workflow.md)
* [Grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md)
* [Tanúsítványok](automation-certificates.md)
* [Kapcsolatok](automation-connections.md)
* [Hitelesítő adatok](automation-credentials.md)
* [Ütemezések](automation-schedules.md)
* [Változók](automation-variables.md)
