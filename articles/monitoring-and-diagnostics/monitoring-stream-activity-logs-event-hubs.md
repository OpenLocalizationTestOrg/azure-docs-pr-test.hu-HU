---
title: "aaaStream hello Azure tevékenységnapló tooEvent hubok |} Microsoft Docs"
description: "Ismerje meg, hogyan toostream hello Azure tevékenységnapló tooEvent hubok."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/06/2017
ms.author: johnkem
ms.openlocfilehash: 336f92771b9d4379ad9dbcadc6997dfae7fae7bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stream-hello-azure-activity-log-tooevent-hubs"></a>Adatfolyam-hello Azure tevékenységnapló tooEvent hubok
Hello [ **Azure tevékenységnapló** ](monitoring-overview-activity-logs.md) továbbítható a közel valós idejű tooany alkalmazások hello portálon hello beépített "Exportálás" beállítás használatával, vagy engedélyezésével hello a Service Bus szabályazonosító keresztül hello napló profil Az Azure PowerShell-parancsmagok vagy Azure CLI-t.

## <a name="what-you-can-do-with-hello-activity-log-and-event-hubs"></a>Mi mindent hello tevékenységnapló és az Event Hubs
Az alábbiakban néhány módokon használhatja a streaming hello tevékenységnapló funkció hello:

* **Adatfolyam-toothird-naplózás és telemetriai rendszerekre** – adott idő alatt, az Event Hubs streaming lesz a tevékenységnapló hello mechanizmus toopipe válnak a külső siem-ektől és jelentkezzen elemzési megoldásokat.
* **Egy egyéni telemetriai adatokat, illetve a naplózási platform** – Ha már rendelkezik egy egyedi telemetriai platform vagy a rendszer csak a gondolat egy kiválóan méretezhető hello közzétételi-feliratkozási épület jellegétől függően az Event Hubs lehetővé teszi a tooflexibly betöltési hello Tevékenységnapló. [Dan Rosanova útmutató toousing Event Hubs egy globális méretű telemetriai platform itt talál.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-hello-activity-log"></a>Hello tevékenységnapló adatfolyamként
Adatfolyamként való küldése a hello tevékenységnapló programozott módon vagy hello portálon keresztül is engedélyezheti. Mindkét módszer esetén, válasszon egy Service Bus Namespace és egy megosztott elérési házirendet a névtérhez, és az Eseményközpontok jön létre az adott névtérben, hello első új tevékenységnapló esemény bekövetkezésekor. Ha nem rendelkezik egy Service Bus Namespace, először egy toocreate. Ha korábban folyamatos átviteli tevékenységnapló események toothis Service Bus Namespace, korábban létrehozott Eseményközpont hello fogja használni. hello megosztott hozzáférési házirend meghatározása hello adatfolyam mechanizmussal rendelkezik hello engedélyeket. Jelenleg tooan Event Hubs streaming van szükség **kezelése**, **küldése**, és **figyelésére** engedélyek. Hozzon létre, vagy módosíthatja a Service Bus Namespace Service Bus Namespace megosztott hozzáférési házirendek a klasszikus portálon hello hello "Beállítása" lapon. tooupdate hello tevékenységnapló napló profil tooinclude streaming, hello módosítás hello felhasználói hello ListKey engedéllyel kell rendelkeznie a Service Bus engedélyezési szabályt.

hello a busz- vagy event hub szolgáltatásnévtér nem rendelkezik a toobe hello hello előfizetési naplók kibocsátó mindaddig, amíg hello beállítás konfiguráló hello felhasználó rendelkezik-e megfelelő hozzáférési tooboth előfizetéseket a Szerepalapú tárolóként ugyanazt az előfizetést.

### <a name="via-azure-portal"></a>Azure-portálon
1. Keresse meg a toohello **tevékenységnapló** panel bal oldalán található hello portal hello a hello menü segítségével.
   
    ![Keresse meg a napló tooActivity portálon](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Kattintson a hello **exportálása** hello panel felső hello gombra.
   
    ![A portál Exportálás gomb](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Hello panelen megjelenő válassza ki, amelynek szeretné toostream események és hello Service Bus Namespace, amelyben az Eseményközpont toobe létrehozott az adatfolyamként történő ezeket az eseményeket szeretné hello régiók.
   
    ![Exportálás tevékenységnapló panel](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Kattintson a **mentése** toosave ezeket a beállításokat. hello-beállítások vannak azonnal kell alkalmazott tooyour előfizetés.

### <a name="via-powershell-cmdlets"></a>PowerShell-parancsmagok használatával
Ha a napló-profilja már létezik, először tooremove adott profil.

1. Használjon `Get-AzureRmLogProfile` tooidentify, ha a napló-profil létezik
2. Ha igen, használjon `Remove-AzureRmLogProfile` tooremove azt.
3. Használjon `Set-AzureRmLogProfile` toocreate profil:

```
Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

Service Bus Szabályazonosító hello egy ilyen formátumú karakterláncot: {service bus erőforrás-azonosító} /authorizationrules/ {kulcsnév}, például 

### <a name="via-azure-cli"></a>Az Azure parancssori felület használatával
Ha a napló-profilja már létezik, először tooremove adott profil.

1. Használjon `azure insights logprofile list` tooidentify, ha a napló-profil létezik
2. Ha igen, használjon `azure insights logprofile delete` tooremove azt.
3. Használjon `azure insights logprofile add` toocreate profil:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Service Bus Szabályazonosító hello egy ilyen formátumú karakterláncot: `{service bus resource ID}/authorizationrules/{key name}`.

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a>Hogyan használnak a hello adatainak naplózása az Event Hubs?
[hello tevékenységnapló hello sémája érhető el itt](monitoring-overview-activity-logs.md). Minden esemény "rekordok." nevű JSON-blobok tömbje van

## <a name="next-steps"></a>Következő lépések
* [Archív hello tevékenységnapló tooa storage-fiók](monitoring-archive-activity-log.md)
* [Olvassa el a hello Azure tevékenységnapló hello áttekintése](monitoring-overview-activity-logs.md)
* [Tevékenységnapló esemény alapján riasztás beállítása](insights-auditlog-to-webhook-email.md)

