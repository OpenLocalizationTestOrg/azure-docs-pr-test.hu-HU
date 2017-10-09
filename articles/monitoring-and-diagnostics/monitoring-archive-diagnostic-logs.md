---
title: "Azure diagnosztikai naplók aaaArchive |} Microsoft Docs"
description: "Ismerje meg, hogyan tooarchive az Azure diagnosztikai naplók a hosszú távú megőrzési tárfiókokban."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 3a55c73f-2ef3-45f3-8956-bcf9c0cb7e05
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: bc9edbd3a649023a728b7fe77130dba2b6e6370d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="archive-azure-diagnostic-logs"></a>Az Azure diagnosztikai naplók archiválása
Ebben a cikkben megmutatjuk, hogyan használhatja a hello Azure-portálon PowerShell-parancsmagok, CLI-t, vagy REST-API tooarchive a [Azure diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md) tárfiókokban. Ez a beállítás akkor hasznos, ha azt szeretné, hogy a diagnosztikai naplók naplózási, statikus elemzési és biztonsági mentés olyan opcionális megőrzési házirend tooretain. hello storage-fiók nem rendelkezik toobe hello naplók kibocsátó mindaddig, amíg hello beállítás konfiguráló hello felhasználó rendelkezik-e megfelelő hozzáférési tooboth előfizetéseket a Szerepalapú hello erőforrás ugyanahhoz az előfizetéshez.

## <a name="prerequisites"></a>Előfeltételek
Kezdés előtt kell túl[hozzon létre egy tárfiókot](../storage/storage-create-storage-account.md) toowhich úgy archiválhatók a diagnosztikai naplókat. Erősen ajánlott, hogy nem használja a benne tárolt, így jobban szabályozhatja a hozzáférést toomonitoring adatok más, nem figyelési adatokat tartalmazó meglévő tárfiókot. Azonban is archiválása a tevékenységnapló és diagnosztikai metrikák tooa tárfiókot, ha azt is ellenőrizze logika toouse, hogy a tárfiók a diagnosztikai naplók, valamint tookeep összes figyelési adatok egy központi helyen. hello tárfiókot használ egy általános célú tárfiókkal, nem a blob storage-fiók kell lennie.

## <a name="diagnostic-settings"></a>Diagnosztikai beállítások
a diagnosztikai naplók hello módszereket az alábbi tooarchive, állítsa be a **diagnosztikai beállításának** egy adott erőforráshoz. Egy erőforrás diagnosztikai beállításának hello kategóriák naplók határozza meg, és metrika adatküldés tooa cél (tárfiók, az Event Hubs névtér vagy Naplóelemzési). Az események minden egyes napló kategória és a storage-fiókban tárolt metrikaadatokat hello adatmegőrzési (tooretain napok száma) is meghatároz. Ha adatmegőrzési toozero, események naplózási kategóriát tárolják határozatlan ideig (az toosay, végtelen). Adatmegőrzési ellenkező esetben lehet bármely 1 és 2147483647 között eltelt napok számát. [További diagnosztikai beállítások itt kapcsolatos](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings). Adatmegőrzési alkalmazott napi, így: hello napi (UTC) szerint végét naplózza, amelyik most már túl hello adatmegőrzési hello nap törlődni fog. Például ha egy nap adatmegőrzési, ma hello nap hello elején hello hello nap előtt tegnapi naplók lennének törölhetők

## <a name="archive-diagnostic-logs-using-hello-portal"></a>Diagnosztikai naplók archiválása hello portál használatával
1. Hello portálon keresse meg a figyelő tooAzure, majd kattintson a **diagnosztikai beállítások**

    ![Figyelés szakaszban Azure-figyelő](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. Opcionálisan hello szűrőlistával erőforráscsoport és erőforrások típus szerint, majd a hello erőforrás diagnosztikai beállításának tooset milyen, amelynek.

3. Ha a beállítások nem található a kiválasztott hello erőforrás, felszólító toocreate beállítás áll. Kattintson a "Diagnosztika bekapcsolásához."

   ![Diagnosztikai beállítás - nincsenek meglévő beállítások hozzáadása](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   Ha meglévő beállítások hello erőforráson, látni fogja a már ehhez az erőforráshoz konfigurált beállítások listáját. A "Hozzáadás diagnosztikai beállításának."

   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. Adja meg a nevének beállítása és hello jelölőnégyzetet a **tooStorage fiók exportálása**, majd válasszon egy tárfiókot. Beállíthatja a nap tooretain számú ezek a naplók hello segítségével **megőrzés (nap)** csúszkák. Egy nulla napos megőrzési hello naplók határozatlan ideig tárolja.
   
   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)
    
4. Kattintson a **Save** (Mentés) gombra.

Néhány másodpercen belül hello új beállítás jelenik meg az ehhez az erőforráshoz beállítások listáját, és diagnosztikai naplók archivált toothat tárolási fiók, amint létrejön az új esemény-adatokat.

## <a name="archive-diagnostic-logs-via-azure-powershell"></a>Diagnosztikai naplók archiválása Azure PowerShell használatával
```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Tulajdonság | Szükséges | Leírás |
| --- | --- | --- |
| ResourceId |Igen |Erőforrás-azonosító hello erőforrás diagnosztikai beállításának tooset kívánja. |
| StorageAccountId |Nem |Erőforrás-azonosítója, hello Tárfiók toowhich diagnosztikai naplók menteni kell. |
| Kategóriák |Nem |Napló kategóriák tooenable vesszővel tagolt listája. |
| Engedélyezve |Igen |Logikai érték, amely jelzi, hogy diagnosztika engedélyezett vagy letiltott ezen az erőforráson. |
| RetentionEnabled |Nem |Logikai érték, amely azt jelzi, hogy engedélyezve vannak-e adatmegőrzési ezen az erőforráson. |
| retentionInDays |Nem |Amely események kell megtartani 1 és 2147483647 közötti napok számát. A nulla érték hello naplók határozatlan ideig tárolja. |

## <a name="archive-diagnostic-logs-via-hello-cross-platform-cli"></a>Diagnosztikai naplók archiválása hello platformfüggetlen parancssori felület használatával
```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Tulajdonság | Szükséges | Leírás |
| --- | --- | --- |
| resourceId |Igen |Erőforrás-azonosító hello erőforrás diagnosztikai beállításának tooset kívánja. |
| storageId |Nem |Erőforrás-azonosító hello Tárfiók toowhich diagnosztikai naplók menteni kell. |
| Kategóriák |Nem |Napló kategóriák tooenable vesszővel tagolt listája. |
| engedélyezve |Igen |Logikai érték, amely jelzi, hogy diagnosztika engedélyezett vagy letiltott ezen az erőforráson. |

## <a name="archive-diagnostic-logs-via-hello-rest-api"></a>Diagnosztikai naplók archiválása hello REST API-n keresztül
[Lásd a jelen dokumentum](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) olvashat, hogyan állíthat be a diagnosztikai beállításának hello Azure figyelő REST API használatával.

## <a name="schema-of-diagnostic-logs-in-hello-storage-account"></a>Diagnosztikai naplók hello tárfiókban sémája
Állított be archiválási, miután egy tároló jön létre hello tárfiók hello napló kategóriák engedélyezte előfordulásakor egy eseményt. hello tárolóban hello BLOB formátuma azonos a diagnosztikai naplók közötti hello és hello tevékenységnapló követése. a blobok hello szerkezete van:

> insights - logs-{napló kategória neve} / resourceId = / ELŐFIZETÉSEK / {előfizetés-azonosító} /RESOURCEGROUPS/ {erőforráscsoport neve} /PROVIDERS/ {erőforrás-szolgáltató neve} / {resource type} / {erőforrás neve} / y = {négyjegyű numerikus year} / m = {kétjegyű numerikus month} / d {kétjegyű számozott napja} = / h = {kétjegyű 24 órás hour}/m=00/PT1H.json
> 
> 

Vagy egyszerűbb,

> insights - logs-{napló kategória neve} / resourceId = / {erőforrás-azonosítót} / y = {négyjegyű numerikus year} / m = {kétjegyű numerikus month} / d {kétjegyű számozott napja} = / h = {kétjegyű 24 órás hour}/m=00/PT1H.json
> 
> 

A blob neve lehet, például:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
> 
> 

Minden egyes PT1H.json blob hello blob URL-címben megadott hello órán belül előforduló eseményeket a JSON blob tartalmaz (például h = 12). Hello jelen óra, során eseményeket hozzáfűzött toohello PT1H.json fájlt is, azok bekövetkezésekor. hello perc (m = 00) mindig 00, mivel diagnosztikai alkalmazásnapló-események az egyes blobok óránként van felosztva.

Hello PT1H.json fájlban tárolja minden esemény hello "rekordok" tömb, a következő formátumban:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Elem neve | Leírás |
| --- | --- |
| time |Hello esemény lett létrehozva, amikor hello Azure szolgáltatás feldolgozása hello idejét jelző időbélyegző megfelelő hello esemény kérelmet. |
| resourceId |Erőforrás-azonosítója, hello érintett erőforrás. |
| operationName |Hello művelet neve. |
| category |Napló kategória hello esemény. |
| properties |Állítsa be a `<Key, Value>` hello esemény részleteinek hello leíró párok (azaz szótárában). |

> [!NOTE]
> hello tulajdonságai és használatának azokat a tulajdonságokat a hello erőforrás függően változhat.
> 
> 

## <a name="next-steps"></a>Következő lépések
* [Elemzés blobok letöltése](../storage/storage-dotnet-how-to-use-blobs.md)
* [Adatfolyam-diagnosztikai naplók tooan Event Hubs névtér](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Tudjon meg többet a diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md)
