---
title: "aaaArchive hello Azure tevékenységnapló |} Microsoft Docs"
description: "Ismerje meg, hogyan tooarchive az Azure tevékenység keresse meg a hosszú távú megőrzési tárfiókokban."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: johnkem
ms.openlocfilehash: 58c6d3a3a31398287f66f76999d48f2942ab5109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="archive-hello-azure-activity-log"></a>Archív hello Azure tevékenységnapló
Ebben a cikkben megmutatjuk, hogyan használhatja a hello Azure-portálon, a PowerShell-parancsmagokkal vagy a platformfüggetlen parancssori felület tooarchive a [ **Azure tevékenységnapló** ](monitoring-overview-activity-logs.md) tárfiókokban. Ez a beállítás akkor hasznos, ha azt szeretné, hogy a hosszabb, mint 90 napra (a teljes hozzáféréssel hello adatmegőrzési) tevékenységnapló naplózásához statikus elemzés, vagy biztonsági mentési tooretain. Ha csak tooretain 90 napig, vagy kevesebb, mint az események nem kell tooset archiválási tooa storage-fiók mentése óta tevékenységnapló események kerülnek be az Azure platformon hello 90 napig engedélyezése archiválás nélkül.

## <a name="prerequisites"></a>Előfeltételek
Kezdés előtt kell túl[hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) úgy archiválhatók a tevékenységnapló toowhich. Erősen ajánlott, hogy nem használja a benne tárolt, így jobban szabályozhatja a hozzáférést toomonitoring adatok más, nem figyelési adatokat tartalmazó meglévő tárfiókot. Azonban is archiválás diagnosztikai naplók és a metrikák tooa tárfiókot, ha azt is ellenőrizze logika toouse, hogy a tárfiók a tevékenység-e jelentkezni, valamint tookeep összes figyelési adatok egy központi helyen. hello tárfiókot használ egy általános célú tárfiókkal, nem a blob storage-fiók kell lennie. hello storage-fiók nem rendelkezik toobe hello hello előfizetési naplók kibocsátó mindaddig, amíg hello beállítás konfiguráló hello felhasználó rendelkezik-e megfelelő hozzáférési tooboth előfizetéseket a Szerepalapú tárolóként ugyanazt az előfizetést.

## <a name="log-profile"></a>Napló-profil
hello beállítása tooarchive hello hello módszereket az alábbi tevékenységnapló, **napló profil** az előfizetéshez. hello napló profil meghatározza az eseményeket, amelyek tárolt vagy a folyamatos átviteli hello típusát és kimenetek hello – tárolási fiók és/vagy az event hub. A storage-fiókban tárolt események hello adatmegőrzési (tooretain napok száma) is meghatároz. Ha hello adatmegőrzési toozero, események határozatlan ideig tárolja. Ellenkező esetben ez állítható tooany értékének 1 és 2147483647 között. Adatmegőrzési alkalmazott napi, így: hello napi (UTC) szerint végét naplózza, amelyik most már túl hello adatmegőrzési hello nap törlődni fog. Például ha egy nap adatmegőrzési, ma hello nap hello elején hello hello nap előtt tegnapi naplók törlése akkor történik meg. [Részletesebb naplófájl kapcsolatos profilok Itt](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile). 

## <a name="archive-hello-activity-log-using-hello-portal"></a>Archív hello tevékenységnapló hello portál használatával
1. Hello portálon kattintson hello **tevékenységnapló** hello bal oldali navigációs hivatkozásra kattintva. Ha egy hivatkozást a hello tevékenységnapló nem látható, kattintson a hello **több szolgáltatások** először hivatkozásra.
   
    ![Keresse meg a tooActivity napló panel](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Hello hello panel felső részén kattintson **exportálása**.
   
    ![Hello Exportálás gombra.](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. Hello paneljén megjelenő jelölőnégyzetet hello a **tooa tárfiók exportálása** , és válasszon egy tárfiókot.
   
    ![A storage-fiók beállítása](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. A hello csúszkát vagy szövegmező definiálja, amelynek tevékenységnapló események kell tartani a tárfiókban lévő napok számát. Az adatok határozatlan ideig őrzi hello tárfiók toohave tetszés szerint állítsa be az a szám toozero.
5. Kattintson a **Save** (Mentés) gombra.

## <a name="archive-hello-activity-log-via-powershell"></a>Archív hello tevékenységnapló PowerShell használatával
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Tulajdonság | Szükséges | Leírás |
| --- | --- | --- |
| StorageAccountId |Nem |Erőforrás-azonosítója hello Tárfiók toowhich tevékenységi naplóit, mentésére. |
| Helyek |Igen |Régiók, amelynek szeretné toocollect tevékenységnapló események vesszővel tagolt listája. Megtekintheti az összes régiók listáját [ezen a lapon felkeresésével](https://azure.microsoft.com/en-us/regions) vagy használatával [hello Azure szolgáltatásfelügyelet REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays |Igen |Az eseményeket meg kell őrizni, 1 és 2147483647 közötti napok számát. A nulla érték hello naplók határozatlan ideig tárolja (végtelen). |
| Kategóriák |Igen |Be kell esemény kategóriák vesszővel tagolt listája. Lehetséges értékek a következők: Olvasás, törlés és művelet. |

## <a name="archive-hello-activity-log-via-cli"></a>Archív hello tevékenységnapló parancssori felület használatával
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Tulajdonság | Szükséges | Leírás |
| --- | --- | --- |
| név |Igen |A napló profil neve. |
| storageId |Nem |Erőforrás-azonosítója hello Tárfiók toowhich tevékenységi naplóit, mentésére. |
| Helyek |Igen |Régiók, amelynek szeretné toocollect tevékenységnapló események vesszővel tagolt listája. Megtekintheti az összes régiók listáját [ezen a lapon felkeresésével](https://azure.microsoft.com/en-us/regions) vagy használatával [hello Azure szolgáltatásfelügyelet REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays |Igen |Az eseményeket meg kell őrizni, 1 és 2147483647 közötti napok számát. A nulla érték határozatlan ideig tárolandó hello naplók (végtelen). |
| Kategóriák |Igen |Be kell esemény kategóriák vesszővel tagolt listája. Lehetséges értékek a következők: Olvasás, törlés és művelet. |

## <a name="storage-schema-of-hello-activity-log"></a>Hello tevékenységnapló tároló sémája
Miután állított be archiválási, tárolót létrejön hello tárfiókot, amint tevékenységnapló esemény bekövetkezésekor. hello blobok hello tárolóban kövesse hello hello tevékenységnapló és diagnosztikai naplók formátuma azonos. a blobok hello szerkezete van:

> insights-működési-naplók/name = alapértelmezett/resourceId = / SUBSCRIPTIONS / {előfizetés-azonosító} / y = {négyjegyű numerikus year} / m = {kétjegyű numerikus month} / d {kétjegyű számozott napja} = / h = {kétjegyű 24 órás hour}/m=00/PT1H.json
> 
> 

A blob neve lehet, például:

> insights-Operational-Logs/Name=default/resourceId=/Subscriptions/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON
> 
> 

Minden egyes PT1H.json blob tartalmazza a hello blob URL-címben megadott hello órán belül előforduló eseményeket JSON blob (pl. h = 12). Hello jelen óra, során eseményeket hozzáfűzött toohello PT1H.json fájlt is, azok bekövetkezésekor. hello perc (m = 00) mindig 00, mivel tevékenységnapló események óránként egyes blobok van felosztva.

Hello PT1H.json fájlban tárolja minden esemény hello "rekordok" tömb, a következő formátumban:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
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
| category |Kategória hello művelet, például. Írási, olvassa el, a műveletet. |
| resultType |eg hello hello eredményének típusa. Sikeres, sikertelen, kezdő |
| resultSignature |Hello erőforrás típusától függ. |
| durationMs |Ezredmásodpercben hello művelet időtartama |
| callerIpAddress |IP-cím hello felhasználó hello művelet, a jogcím vagy a rendelkezésre állás alapján SPN jogcím hajtott végre. |
| correlationId |Általában egy GUID Azonosítót a hello karakterlánc-formátum. Az eseményeket, amelyek megosztása a correlationId tartozik toohello uber műveletet. |
| identity |JSON-blob: hello engedélyezési és a jogcímeket. |
| Engedélyezési |A BLOB RBAC tulajdonságok hello esemény. Hello "action", "szerepkör" és "hatókör" Tulajdonságok általában tartalmazza. |
| szint |Hello esemény szintje. Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes" |
| location |A régióban mely hello helyen történt (vagy globális). |
| properties |Állítsa be a `<Key, Value>` hello esemény részleteinek hello leíró párok (azaz szótárában). |

> [!NOTE]
> hello tulajdonságai és használatának azokat a tulajdonságokat a hello erőforrás függően változhat.
> 
> 

## <a name="next-steps"></a>Következő lépések
* [Elemzés blobok letöltése](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [Adatfolyam-hello tevékenységnapló tooEvent hubok](monitoring-stream-activity-logs-event-hubs.md)
* [Tudjon meg többet a hello műveletnapló](monitoring-overview-activity-logs.md)

