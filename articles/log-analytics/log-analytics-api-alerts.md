---
title: "aaaUsing OMS napló Analytics riasztási REST API"
description: "hello napló Analytics riasztási REST API lehetővé teszi a toocreate és riasztások kezelése a alkotó Operations Management Suite (OMS) szolgáltatáshoz.  Ez a cikk részletesen hello API és néhány példa a másik műveletet hajt végre."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 418dc7eb71d6151c6380b8925f1f147a0e13b178
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>Hozzon létre, és a REST API-val Naplóelemzési riasztási szabályok kezelése
hello napló Analytics riasztási REST API lehetővé teszi a toocreate, és kezelheti a riasztásokat az Operations Management Suite (OMS).  Ez a cikk részletesen hello API és néhány példa a másik műveletet hajt végre.

hello napló Analytics Search REST API RESTful és hello Azure Resource Manager REST API-n keresztül érhető el. Ebben a dokumentumban található példák az hello API elérését egy PowerShell parancssori használatával [ARMClient](https://github.com/projectkudu/ARMClient), egy nyílt forráskódú parancssori eszköz, amely leegyszerűsíti a meghívása hello Azure Resource Manager API-val. hello ARMClient és a PowerShell használata sok beállítások tooaccess hello napló Analytics Search API közül. Ezekkel az eszközökkel hello Azure Resource Manager RESTful API toomake hívások tooOMS munkaterületek használják, és hajtsa végre a keresési parancsok rajtuk. hello API kimeneteként keresési eredmények tooyou JSON formátumban, így toouse hello keresési eredmények között számos különböző módon programozott módon.

## <a name="prerequisites"></a>Előfeltételek
Jelenleg riasztásokat csak hozhatja létre a Naplóelemzési mentett keresés.  Olvassa el a toohello [napló Search REST API](log-analytics-log-search-api.md) további információt.

## <a name="schedules"></a>Ütemezések
A mentett kereséseket rendelkezhet egy vagy több ütemezés. hello ütemezés meghatározása milyen gyakran hello keresési fut, és hello időtartam, mely hello feltételek keresztül azonosítja.
Ütemezés a következő táblázat hello hello tulajdonságokkal rendelkezik.

| Tulajdonság | Leírás |
|:--- |:--- |
| időköz |Milyen gyakran hello keresési fut. Percben értendő. |
| queryTimeSpan |mely hello feltételek kiértékelése hello időintervallumban. Nagyobbnak kell lennie mint tooor időköz. Percben értendő. |
| Verzió |hello API-verziót használja.  Jelenleg ez mindig meg kell too1. |

Vegye figyelembe például egy esemény lekérdezés Timespan érték 30 perc és 15 perces időközönként. Ebben az esetben hello lekérdezés futna 15 percenként, és a riasztás akkor váltódik ki, ha hello feltételek keresztül a 30 perces span tooresolve tootrue továbbra is.

### <a name="retrieving-schedules"></a>Ütemezés lekérése
Használjon hello Get metódus tooretrieve összes ütemezés a mentett keresés.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Használjon hello lekérése egy ütemezés azonosító tooretrieve metódust egy meghatározott ütemezést, a mentett keresés.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Az alábbiakban az ütemezett mintát választ.

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a>Ütemezés létrehozása
Hello Put metódust használ egy egyedi ütemezés azonosító toocreate új ütemezést.  Vegye figyelembe, hogy két ütemezések nem rendelkezik hello ugyanazzal az Azonosítóval akkor is, ha különböző társított mentett kereséseket.  Amikor hello OMS konzolban hozna létre egy ütemezést, GUID jön létre hello ütemezési azonosítóját.

> [!NOTE]
> minden mentett keresések, ütemezéseihez és hello napló Analytics API létre műveletek hello nevét kisbetűs kell lennie.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Az ütemezés módosítása
Meglévő ütemezés azonosítója megegyezik a mentett hello keresni, amelyek ütemezése toomodify hello Put metódust használja.  hello kérelem törzse hello hello etag hello ütemterv tartalmaznia kell.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Ütemezés törlése
Egy ütemezés azonosító toodelete ütemezés hello Delete metódust használja.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Műveletek
Ütemezés rendelkezhet több művelet. Egy műveletet határozhatnak meg például a levél küldése vagy a runbook indítása egy vagy több folyamatok tooperform, vagy azt határozhatnak meg a küszöbérték, amely azt határozza meg, ha a keresési eredmények hello bizonyos feltételeknek megfelelő.  Bizonyos műveleteket határozza meg az is, hogy hello folyamatok végrehajtása közben hello küszöbérték elérése.

Minden művelet a következő táblázat hello hello jellemzőkkel rendelkezik.  Különböző típusú riasztások különböző további tulajdonságokat, amelyek az alábbiakban található rendelkezik.

| Tulajdonság | Leírás |
|:--- |:--- |
| Típus |Hello művelet típusa.  Jelenleg hello lehetséges értékei a riasztás és Webhook. |
| Név |Hello riasztás megjelenített neve. |
| Verzió |hello API-verziót használja.  Jelenleg ez mindig meg kell too1. |

### <a name="retrieving-actions"></a>Műveletek végrehajtása
Használata hello Get metódus tooretrieve ütemezett összes műveletet.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Használjon hello beolvasása hello művelet azonosítója tooretrieve metódust egy adott művelet egy ütemezéshez.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Létrehozásával vagy szerkesztésével műveletek
Hello Put metódust használja, amely egyedi toohello ütemezés toocreate új művelet azonosítójú művelet.  Hello OMS konzol egy műveletet hoz létre, amikor egy GUID van hello művelet azonosítóját.

> [!NOTE]
> minden mentett keresések, ütemezéseihez és hello napló Analytics API létre műveletek hello nevét kisbetűs kell lennie.

Egy meglévő művelet azonosítója megegyezik a mentett hello keresni, amelyek ütemezése toomodify hello Put metódust használja.  hello kérelem törzse hello hello etag hello ütemterv tartalmaznia kell.

hello kérés formátuma új művelet létrehozásához művelettípus függ, ezek a példák hello lentebbi szerepelnek a.

### <a name="deleting-actions"></a>Műveletek törlése
Hello művelet azonosítója toodelete művelet hello Delete metódust használja.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Értesítési műveletek
Ütemezés rendelkeznie kell egy műveletet.  Értesítési műveletek legalább a következő táblázat hello hello részeiből.  A további részletek az alábbi leírása.

| A szakasz | Leírás |
|:--- |:--- |
| Küszöbérték |Feltételek hello művelet futási idejét. |
| EmailNotification |Levélküldés toomultiple címzetteknek. |
| Szervizkiszolgáló |Elindít egy forgatókönyvet az Azure Automation tooattempt toocorrect azonosított problémát. |

#### <a name="thresholds"></a>Küszöbértékek
Egy műveletet rendelkeznie kell egy küszöbértéket.  Amikor hello mentett keresés eredményeit, hogy a keresés társított művelet hello küszöbértéke egyeznek, a művelet az egyéb folyamatok futnak.  Egy műveletet is tartalmazhat, csak a küszöbértéket, hogy más típusú, amelyek nem tartalmazzák a küszöbértékek műveletekhez használható.

Küszöbértékek a következő táblázat hello hello tulajdonságokkal rendelkezik.

| Tulajdonság | Leírás |
|:--- |:--- |
| Operátor |Hello küszöbérték összehasonlító operátort. <br> gt = nagyobb mint <br> lt = kisebb, mint |
| Érték |Hello küszöb értékét. |

Vegye figyelembe például 15 perc, 30 perc Timespan és egy nagyobb, mint 10 küszöbértéket időközzel eseménylekérdezési. Ebben az esetben hello lekérdezés futna 15 percenként, és a riasztás akkor váltódik ki, ha 10 keresztül a 30 perces span létrehozott események által visszaadott.

Az alábbiakban látható egy minta válasz csak a küszöbérték a művelet.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Egy egyedi művelet azonosítója toocreate új küszöbérték művelet ütemezett hello Put metódust használata.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Egy meglévő művelet azonosítója toomodify egy küszöbértéket művelet ütemezett hello Put metódust használata.  hello kérelem törzse hello hello etag hello művelet tartalmaznia kell.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-mail értesítések
Értesítő e-mailek küldése mail tooone vagy a címzetteket.  A következő táblázat hello tartoznak hello tulajdonságait.

| Tulajdonság | Leírás |
|:--- |:--- |
| Címzettek |E-mail címek listája. |
| Tárgy |hello hello e-mail tárgyát. |
| Melléklet |Mellékletek jelenleg nem támogatottak, ezért ez a "None" értéke mindig lesz. |

Az alábbiakban látható egy minta választ, a küszöbérték az e-mail értesítési művelet.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is hello subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Egy egyedi művelet azonosítója toocreate egy új e-mail művelet ütemezett hello Put metódust használata.  hello alábbi példa hoz létre a e-mailben értesítést a küszöbérték, üdvözlő levelek küldi, ha a mentett keresés hello hello eredményeit meghaladnia hello küszöbértéket.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

Egy meglévő művelet azonosítója toomodify e-mail művelet ütemezett hello Put metódust használata.  hello kérelem törzse hello hello etag hello művelet tartalmaznia kell.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a>Szervizelési műveletek
Szervizelt érintő toocorrect hello probléma hello riasztás által azonosított Azure Automation forgatókönyv indítása.  Hello runbook egy szervizelési művelet szerepel a webhook létrehozása kell, és adja meg a hello WebhookUri tulajdonság hello URI.  Ez a művelet hello OMS-konzollal létrehozásakor egy új webhook hello runbook automatikusan létrejön.

Szervizelt hello a következő táblázat tartalmazza az hello tulajdonságokat.

| Tulajdonság | Leírás |
|:--- |:--- |
| RunbookName |Hello runbook neve. Ennek egyeznie kell a közzétett runbookok hello Automation-megoldás az OMS-munkaterület a konfigurált hello automation-fiókban. |
| WebhookUri |Hello webhook URI Azonosítóját. |
| A lejárati |hello lejárati dátuma és időpontja hello webhook.  Ha hello webhook egy lejárati nem rendelkezik, majd ez lehet bármely érvényes jövőbeni dátum. |

Az alábbiakban látható egy mintaválasz szervizelési művelethez a küszöbértéket.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Egy egyedi művelet azonosítója toocreate egy új szervizelési művelet ütemezett hello Put metódust használata.  hello alábbi példa hoz létre egy szervizelés a küszöbérték, a mentett keresés hello hello eredményeit-nál nagyobb hello küszöbérték hello runbook indítását.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Egy meglévő művelet azonosítója toomodify a szervizelési művelet ütemezett hello Put metódust használata.  hello kérelem törzse hello hello etag hello művelet tartalmaznia kell.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Példa
Az alábbiakban látható egy teljes példa toocreate egy új e-mail-riasztások.  Ezzel létrehoz egy új ütemezést tartalmazó egy küszöbértéket és e-mailek művelet együtt.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhookműveletek
Webhookműveletek egy folyamat megkezdéséhez hívja az egy URL-cím és a nem kötelezően egy hasznos toobe küldött.  Hasonló tooRemediation műveletek kivételével ezek célja a webhookokkal, előfordulhat, hogy aktiválják az Azure Automation-runbook eltérő folyamatok.  Hello további lehetőség a tartalom céleszközökre toobe toohello távoli folyamattal is biztosítanak.

Webhookműveletek nem rendelkezik a küszöbérték, de ehelyett hozzá kell adni egy riasztási küszöbértéket műveletek tooa ütemezés.  Több webhookműveletek összes futtatni kívánt hello küszöbérték teljesülésekor adhat hozzá.

Webhookműveletek hello a következő táblázat tartalmazza az hello tulajdonságokat.

| Tulajdonság | Leírás |
|:--- |:--- |
| WebhookUri |hello hello e-mail tárgyát. |
| customPayload |Egyéni adattartalom küldött toobe toohello webhook.  hello formátum függvényében adható meg, milyen hello webhook által várt paraméterekkel. |

Az alábbiakban látható egy mintaválasz webhook műveletet és egy társított műveletet a küszöbértéket.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Hozzon létre vagy egy webhook művelet szerkesztése
Egy egyedi művelet azonosítója toocreate új webhook művelet ütemezett hello Put metódust használata.  hello alábbi példa létrehoz egy Webhook műveletet és egy műveletet a küszöbértéket, hello webhook váltja hello mentett keresés eredményei hello meghaladnia hello küszöbértéket.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Egy meglévő művelet azonosítója toomodify egy webhook művelet ütemezett hello Put metódust használata.  hello kérelem törzse hello hello etag hello művelet tartalmaznia kell.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Következő lépések
* Használjon hello [REST API tooperform napló keresések](log-analytics-log-search-api.md) a Naplóelemzési.

