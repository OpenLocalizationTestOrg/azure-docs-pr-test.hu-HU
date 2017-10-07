---
title: "Azure Automation forgatókönyv aaaTesting |} Microsoft Docs"
description: "Runbook közzététele az Azure Automationben, mielőtt a teszteléshez le tooensure, hogy megfelelően működik-e.  Ez a cikk ismerteti, hogyan tootest egy runbook és tekintheti meg a kimenetét."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 7f7db785-52c0-4613-aa12-b02fd32a5182
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 8c531f702699d586f8215d4c171cb0ecf94732b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="testing-a-runbook-in-azure-automation"></a>Runbook tesztelése az Azure Automationben
Egy runbook tesztelésekor hello [vázlatként megjelölt verziót](automation-creating-importing-runbook.md#publishing-a-runbook) végrehajtása és minden elvégzett műveletet végrehajt befejezését. Nincs feladatelőzményekben létrejött, de hello [kimeneti](automation-runbook-output-and-messages.md#output-stream) és [figyelmeztetés és hiba](automation-runbook-output-and-messages.md#message-streams) adatfolyamok megjelennek a hello teszt kimeneti ablaktáblában. Üzenetek toohello [részletes adatfolyam](automation-runbook-output-and-messages.md#message-streams) hello Tesztkimenet ablaktáblán megjelennek, csak ha hello [$VerbosePreference változó](automation-runbook-output-and-messages.md#preference-variables) tooContinue van beállítva.

Annak ellenére, hogy hello vázlatként megjelölt verziót futtatása, a hello runbook továbbra is hello munkafolyamat végrehajtása általában és erőforrások minden műveletet elvégez hello környezetben. Ezért csak nem éles erőforrások mappaszinten kell tesztelni.

az eljárás tootest hello [típusú forgatókönyvet](automation-runbook-types.md) hello azonos, és a tesztelése hello szöveges editor és hello grafikus szerkesztő hello Azure-portálon a között nincs különbség.  

## <a name="tootest-a-runbook-in-hello-azure-portal"></a>tootest egy runbookot a hello Azure-portálon
Használhat egyetlen [runbooktípusba](automation-runbook-types.md) a hello Azure-portálon.

1. Nyissa meg hello vázlatként megjelölt verziót vagy hello hello runbook [szöveges szerkesztő](automation-edit-textual-runbook.md) vagy [grafikus szerkesztő](automation-graphical-authoring-intro.md).
2. Kattintson a hello **teszt** gomb tooopen hello teszt panelen.
3. Ha hello runbook paraméterekkel rendelkezik, azok hello bal oldali ablaktáblán, ahol megadhatja a hello vizsgálathoz használt értékek toobe megjelennek.
4. Ha azt szeretné, toorun hello tesztelési meg egy [hibrid forgatókönyv-feldolgozó](automation-hybrid-runbook-worker.md), majd módosítsa **futtatása beállítások** túl**Hibridfeldolgozó** és select hello hello célcsoport neve.  Ellenkező esetben hagyja az alapértelmezett hello **Azure** toorun hello teszt hello felhőben.
5. Kattintson a hello **Start** gomb toostart hello tesztelése.
6. Ha hello runbook [PowerShell munkafolyamat](automation-runbook-types.md#powershell-workflow-runbooks) vagy [grafikus](automation-runbook-types.md#graphical-runbooks), akkor állítsa le, vagy amíg hello gombok hello kimeneti ablaktábla alatti tesztelését felfüggeszti. Hello runbook felfüggesztésekor hello aktuális tevékenység befejezése felfüggesztés előtt. Miután hello runbook fel van függesztve, állítsa le, vagy indítsa újra.
7. Vizsgálja meg a hello kimeneti hello runbookból hello kimenet ablaktáblán.

## <a name="next-steps"></a>Következő lépések
* Hogyan toocreate vagy egy runbook importálása: toolearn [létrehozása vagy egy Azure Automation forgatókönyv importálása](automation-creating-importing-runbook.md)
* toolearn grafikus szerzői kapcsolatos további információkért lásd: [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md)
* a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md)
* toolearn bővebben runboks tooreturn állapotüzeneteket és a hibák, beleértve az ajánlott eljárások konfigurálásáról lásd: [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md)

