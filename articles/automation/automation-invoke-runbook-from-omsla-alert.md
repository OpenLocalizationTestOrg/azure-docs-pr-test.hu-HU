---
title: "egy Azure Automation-Runbook a napló Analytics riasztási aaaCalling |} Microsoft Docs"
description: "Ez a cikk áttekintést hogyan tooinvoke egy Automation-runbook Microsoft OMS Log Analytics-riasztás alapján."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/31/2017
ms.author: magoedte
ms.openlocfilehash: 8b745d6e6c2b0294d676e010f52855cd51741cf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="calling-an-azure-automation-runbook-from-an-oms-log-analytics-alert"></a>Azure Automation-runbook hívása OMS Log Analytics-riasztásból

Ha riasztás konfigurálva van a Naplóelemzési toocreate sikertelen lesz, egy riasztás rekordot, ha az eredmények feltételeknek egy adott, például a hosszan tartó csúcs az igények processzorhasználat vagy egy adott alkalmazás folyamat kritikus toohello funkciói az üzleti alkalmazások és egy kapcsolódó esemény Windows eseménynaplóban hello, riasztás automatikusan futtathat egy Automation-runbook egy kísérlet tooauto-hello probléma megoldásáról.  

Nincsenek két beállítások toocall runbook hello riasztás konfigurálásakor.  Ezek:

1. Egy webhook használata.
   * Ez hello egyetlen elérhető esetén pedig az OMS-munkaterület nem csatolt tooan Automation-fiók.
   * Ha már rendelkezik egy Automation-fiók kapcsolódó tooan OMS-munkaterület, a beállítás akkor továbbra is elérhető.  

2. A runbook közvetlen kiválasztása.
   * Ez a beállítás csak a csatolt tooan Automation-fiók hello OMS-munkaterület esetén érhető el.  

## <a name="calling-a-runbook-using-a-webhook"></a>Runbook meghívása webhook használatával

A webhook lehetővé teszi egy adott runbookhoz toostart az Azure Automationben keresztül egyetlen HTTP-kérelem.  Hello konfigurálása előtt [Naplóelemzési riasztás](../log-analytics/log-analytics-alerts.md#alert-rules) toocall hello runbook egy webhook használatával riasztási művelet, szüksége lesz toofirst hello runbook, amelyeket a rendszer ezzel a módszerrel a webhook létrehozása.  Tekintse át és kövesse hello hello [a webhook létrehozása](automation-webhooks.md#creating-a-webhook) a következő cikket, és ne feledje toorecord hello webhook URL-CÍMÉT, hogy hivatkozhasson rá hello riasztási szabály konfigurálása közben.   

## <a name="calling-a-runbook-directly"></a>Runbookok közvetlen meghívása

Ha rendelkezik hello Automation & vezérlő ajánlat telepített, és az OMS-munkaterület konfigurált hello Runbook műveletek beállítás hello riasztás konfigurálásakor, megtekintheti az összes runbookokat hello **válasszon ki egy runbookot** legördülő lista és válassza ki azt szeretné, hogy a válasz toohello riasztás toorun hello adott runbook.  kijelölt hello runbook futhat a hello Azure-felhőbe vagy egy hibrid forgatókönyv-feldolgozót a munkaterületen.  Amikor hello riasztást hoz létre hello runbook beállítás használatával, a webhook hello runbook hoz létre.  Ha nyissa meg toohello Automation-fiók, és keresse meg a toohello webhook panelen kiválasztott hello runbook hello webhook tekintheti meg.  Ha törli a hello riasztást, hello webhook nem törlődik, de hello felhasználó manuálisan törölheti hello webhook.  Nincs probléma Ha hello webhook nem törlődik, hogy csak egy árva elemet, végül kell toobe rendelés toomaintain szervezett Automation-fiók törlése.  

## <a name="characteristics-of-a-runbook-for-both-options"></a>A runbookok jellemzői (mindkét lehetőség esetében)

Mindkét módszer esetén hello runbook meghívása hello Log Analytics-riasztás alapján másképp viselkednek tulajdonságokkal rendelkeznek, a riasztási szabályok konfigurálása előtt tisztában toobe kell rendelkeznie.  

* Rendelkeznie kell egy **WebhookData** elnevezésű és **Objektum** típusú bemeneti runbookparaméterrel.  Ez lehet kötelező vagy nem kötelező.  hello riasztás hello keresési eredmények toohello runbook a bemeneti paraméter használatával továbbítja.

        param  
         (  
          [Parameter (Mandatory=$true)]  
          [object] $WebhookData  
         )

*  Kód tooconvert hello WebhookData tooa PowerShell objektummal kell rendelkeznie.

    `$SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value`

    *$SearchResults* objektumokból álló tömb lesz; az egyes objektumok hello értéket tartalmazó mező az egy keresési eredményből.

### <a name="webhookdata-inconsistencies-between-hello-webhook-option-and-runbook-option"></a>Hello webhook és runbook lehetőség WebhookData ellentmondásainak

* Egy riasztás toocall olyan Webhook konfigurálásakor adja meg a runbookhoz létrehozott webhook URL-címet, majd kattintson a hello **teszt Webhook** gombra.  hello eredményül kapott WebhookData küldött toohello runbook nem tartalmaz vagy *. SearchResult* vagy *. SearchResults*.

*  Riasztás, ha hello riasztás váltja ki, és meghívja a hello webhook menti, ha WebhookData küldött toohello runbook hello tartalmaz *. SearchResult*.
* Ha riasztás létrehozása, és konfigurálja úgy a runbook (amely is létrehoz egy webhook) toocall, amikor hello WebhookData toohello runbook tartalmazó küld riasztási eseményindítók *. SearchResults*.

Így a hello kód a fenti példában kell tooget *. SearchResult* Ha hello riasztás meghívja a webhook, és tooget kell *. SearchResults* Ha hello riasztás egy runbook közvetlenül.

## <a name="example-walkthrough"></a>Forgatókönyv – példa

Bemutatjuk, hogyan működik a következő példa grafikus forgatókönyv, amely egy Windows-szolgáltatás elindul hello segítségével.<br><br> ![Windows-szolgáltatás grafikus runbookjának indítása](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)<br>

hello runbook rendelkezik egy bemeneti paraméter típusú **objektum** neve, amely **WebhookData** , és tartalmazza a hello riasztási tartalmazó átadott hello webhook adatok *. SearchResults*.<br><br> ![Runbook bemeneti paraméterei](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)<br>

Az ebben a példában a Naplóelemzési létrehozott két egyéni mezők *SvcDisplayName_CF* és *SvcState_CF*, tooextract hello szolgáltatás megjelenített neve és hello hello szolgáltatás állapota (azaz fut vagy leállt) hello eseményből származó írt toohello rendszer-eseménynaplójában.  A Microsoft majd riasztási szabályt létrehozni a következő keresési lekérdezés hello: `Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"` , hogy azt észleli, ha hello nyomtatásisor-kezelő szolgáltatás le van állítva, a rendszer a Windows hello.  Minden szolgáltatás egyik fontos lehet, de ehhez a példához jelenleg hivatkozik egy már meglévő szolgáltatások hello hello Windows operációs rendszer részét képező.  hello riasztási művelet konfigurált tooexecute a runbook ebben a példában használt, és futtassa a hello hibrid forgatókönyv-feldolgozó, amelyek engedélyezve vannak a hello célrendszereket.   

a runbook-kód tevékenység hello **LA az beszerzése szolgáltatás neve** fog átalakítása hello JSON-formátumú karakterláncot egy objektumtípus és hello elemet a szűrő *SvcDisplayName_CF* tooextract hello megjelenített neve a hello A Windows szolgáltatás, és adja át ennek alakzatot hello következő tevékenység, amely ellenőrzi, hogy hello szolgáltatás le van állítva, mielőtt megpróbálná toorestart azt.  *SvcDisplayName_CF* van egy [egyéni mező](../log-analytics/log-analytics-custom-fields.md) Naplóelemzési toodemonstrate ebben a példában létrehozott.

    $SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value
    $SearchResults.SvcDisplayName_CF  

Hello szolgáltatás leállásakor hello riasztási szabály a Naplóelemzési felismeri egyezés és kezdeményezi hello runbook és hello riasztás környezete toohello runbook küldése. hello runbook műveletet nem hajt végre tooverify hello szolgáltatás leáll, és ha így kísérlet toorestart hello szolgáltatást, és győződjön meg arról, hogy megfelelően elindult-e, és kimeneti hello annak az eredménye.     

Másik lehetőségként az Automation-fiókhoz kapcsolódó tooyour OMS-munkaterület nincs, ha szeretné hello riasztási szabály konfigurálása egy webhook művelet tootrigger hello runbookhoz és hello runbook tooconvert hello JSON-formátumú karakterláncot és a szűrőkonfigurálása*. SearchResult* a korábban említett hello útmutatást követve.    

## <a name="next-steps"></a>Következő lépések

* Naplóelemzési a riasztásokkal kapcsolatos további toolearn és hogyan toocreate, lásd: [riasztásokat a Naplóelemzési](../log-analytics/log-analytics-alerts.md).

* Hogyan tootrigger runbookok egy webhook: toounderstand [Azure Automation-webhook](automation-webhooks.md).
