---
title: "aaaUser által kezdeményezett Azure Automation Runbook műveletet a Naplóelemzési |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogy egy Automation-runbook a Log Analyticshez való toorun a keresési eredmény igény."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 53c25431572babd5fd54bf964e4683077e2a4c2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>A Naplóelemzési napló keresési eredmény egy Automation-Runbook a művelet végrehajtása

Az Azure Naplóelemzés napló keresési eredményt, most kiválaszthatja **hajtsa végre a műveletet** toorun egy Automation-runbook.  hello runbook felhasználható tooremediate probléma hello igénybe vehet néhány más művelet, például hibaelhárítási információk gyűjtése, küldjön egy e-mailt vagy szolgáltatási kérelem létrehozása. 

## <a name="components-and-features-used"></a>Használt összetevők és szolgáltatások
* [Azure Automation-fiók](../automation/automation-offering-get-started.md)
* [A Naplóelemzési munkaterület](../log-analytics/log-analytics-overview.md)

## <a name="tooinitiate-runbook-from-log-search"></a>naplófájl-keresési tooinitiate runbookot

tootake művelet egy esemény és a napló a keresési eredmények runbook indítása, akkor először hozzon létre egy naplófájl-keresési, és a hello eredmények hívhat meg a runbook igény.  Ez a napló keresési funkciója hello hello Azure érhető el vagy [OMS-portálon](../log-analytics/log-analytics-log-searches.md).  Ebben a példában a naplófájl-keresési hello Azure portál, amely a szolgáltatás alapvető bemutatója a végezzük.

1. Hello Azure-portálon, hello központ menüben kattintson a **további szolgáltatások** válassza **Naplóelemzési**.  
2. Hello Naplóelemzési paneljén válassza ki a Naplóelemzési munkaterület, majd hello munkaterület panelen **naplófájl-keresési**.  
3. Hajtsa végre a naplófájl-keresési hello napló keresése panelen.  
4. Hello napló a keresési eredmények, kattintson a hello ellipszis toohello balra hello mezők és hello előugró ablakban válassza ki az egyik **művelet végrehajtása a**.<br><br> ![Válassza a keresési eredmény érvénybe műveletet](./media/log-analytics-log-search-takeaction/log-search-takeaction-menuoption.png) 
5. Hello hajtsa végre a megfelelő művelet panelen válassza ki **egy runbook futtatására**, és a hello **egy runbook futtatására** panelen kiválaszthatja, hogy egy runbook toorun.  Igény szerint minden olyan forgatókönyvben a hello csatolt toohello Naplóelemzési munkaterület Automation-fiók.  Vegye figyelembe a következőket hello:

    * Runbookok címkék alapján vannak rendezve.
    * Runbook bemeneti paraméterértékek közvetlenül a hello mezők hello keresési eredmény lehet kiválasztani.  A legördülő lista megjelenik az összes hello választható mezők hello eredmény tooselect a.  
    * Választhat a toorun hello runbook egy [hibrid forgatókönyv-feldolgozó](../automation/automation-hybrid-runbook-worker.md) hogy hello hibát tartalmaz, ha a megfelelő hibrid forgatókönyv-feldolgozó csoport tagja a gép csak tartalmazó hello gépen telepítve van.  Ha a hibrid feldolgozócsoport hello hello neve megegyezik a hello lévő számítógépet észlel hello napló keresési eredmény hello nevét, majd hello csoport automatikusan ki van választva.    

6. Miután rákattintott **futtassa**, megnyílik a hello runbook feladat panel tooallow meg tooreview hello hello feladat állapota.   

Ha egy runbookot, de a beállított toobe [Log Analytics-riasztás alapján nevű](../automation/automation-invoke-runbook-from-omsla-alert.md), van egy bemeneti paraméter, **WebhookData** , amely **objektum** típusa.  Ha hello bemeneti paraméter megadása kötelező, meg kell toopass hello keresési eredmények toohello runbook, akkor átalakíthatja hello JSON-formátumú karakterlánc, amelyre hivatkozhat a runbook-tevékenységek elemeket a toofilter lehetővé objektumtípust.  Ehhez kiválasztásával **keresési eredmények (objektum)** hello legördülő listából.<br><br> ![Válassza ki a Webhook-objektumot a runbook paraméter](media/log-analytics-log-search-takeaction/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Következő lépések

* Felülvizsgálati hello [Naplóelemzési jelentkezzen keresési hivatkozás](log-analytics-search-reference.md) tooview összes hello keresési mezők és facets Naplóelemzési érhető el.
* Hogyan tooinvoke egy Automation-runbook automatikusan, tekintse át toolearn [egy Azure Automation-runbook hívja az OMS-Log Analytics-riasztás alapján](../automation/automation-invoke-runbook-from-omsla-alert.md).  
