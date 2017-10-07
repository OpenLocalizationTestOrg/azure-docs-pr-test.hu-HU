---
title: "aaaUse automatikus skálázási műveletek toosend e-mailek és a webhook riasztási értesítéseket. | Microsoft Docs"
description: "Lásd: hogyan toouse automatikus skálázási műveletek toocall webalkalmazás URL-címeket vagy e-mail értesítéseket küldhet az Azure-figyelő. "
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: eb9a4c98-0894-488c-8ee8-5df0065d094f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: ancav
ms.openlocfilehash: f611a18f5a808412fbdd0c89e3addb36437064c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-autoscale-actions-toosend-email-and-webhook-alert-notifications-in-azure-monitor"></a>Automatikus skálázási műveletek toosend e-mailek és a webhook riasztási értesítések használata az Azure-figyelő
Ez a cikk bemutatja, hogyan lehet beállítani az eseményindítók, hogy az adott webes URL-címek hívja, vagy az Azure-ban automatikus skálázási műveletek alapján a e-mailek küldése.  

## <a name="webhooks"></a>webhook
Webhook tooroute hello Azure riasztási értesítések tooother rendszerek utófeldolgozási vagy egyéni értesítések engedélyezése. Például egy bejövő webes kérelem toosend SMS, napló hibák kezelhető útválasztási hello riasztási tooservices értesítés Csevegés használata vagy üzenetküldési szolgáltatások csoport, stb. hello webhook URI kell lennie egy érvényes HTTP vagy HTTPS-végponton.

## <a name="email"></a>E-mail cím
E-mail küldhető tooany érvényes e-mail címet. A rendszergazdák és a társadminisztrátorok hello szabály futtató hello előfizetés is értesítést fog kapni.

## <a name="cloud-services-and-web-apps"></a>Cloud Services és a Web Apps
Ön részt hello Azure-portálon a felhőalapú szolgáltatásokhoz és a kiszolgálófarmok (webalkalmazások).

* Válassza ki a hello **szerint** metrikát.

![Bővítse a](./media/insights-autoscale-to-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a>Virtuálisgép-méretezési csoportok
Újabb virtuális gépek létrehozása a Resource Manager (virtuálisgép-méretezési csoportok) konfigurálhatja a REST API-t, a Resource Manager sablonok, a PowerShell és a parancssori felület használatával. A portál felület még nem érhető el.
Ha hello REST API-t vagy a Resource Manager-sablon, vegye fel a hello értesítések elem az alábbi beállítások hello.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
| Mező | Kötelező? | Leírás |
| --- | --- | --- |
| művelet |igen |Az értéknek kell lennie a "Méretezési" |
| sendToSubscriptionAdministrator |igen |érték lehet "true" vagy "false" |
| sendToSubscriptionCoAdministrators |igen |érték lehet "true" vagy "false" |
| customEmails |igen |érték lehet null értékű [] vagy e-mailek karakterlánc tömbje |
| webhook |igen |értéke lehet null vagy érvénytelen Uri |
| serviceUri |igen |egy érvényes https Uri |
| properties |igen |érték üres {} kell lennie, vagy tartalmazhat kulcs-érték párok |

## <a name="authentication-in-webhooks"></a>A webhook hitelesítés
hello webhook hitelesítheti a jogkivonat-alapú hitelesítés használatát, mentési helyét hello webhook URI lekérdezési paraméterként jogkivonat-azonosítóval. Például https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue

## <a name="autoscale-notification-webhook-payload-schema"></a>Automatikus skálázás értesítési webhook hasznos séma
Hello automatikus skálázás értesítést generál, amikor hello következő metaadatokat tartalmazza hello webhook hasznos:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' toocapacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| Mező | Kötelező? | Leírás |
| --- | --- | --- |
| status |igen |hello állapot jelzi, hogy létrejött-e egy automatikus skálázási művelet |
| művelet |igen |A példányok növelését "Horizontális Felskálázás" lesz, és példányok csökkenése, lesz az "Méretezési a" |
| A környezetben |igen |hello automatikus skálázási művelet környezetben |
| időbélyeg |igen |Időbélyegzőt, ha hello automatikus skálázási művelet lett elindítva. |
| id |Igen |Erőforrás-kezelő azonosító hello automatikus skálázási beállítás |
| név |Igen |hello hello automatikus skálázási beállítás neve |
| Részletek |Igen |Tartott magyarázatát, hogy az automatikus skálázás szolgáltatás hello hello műveletet, és a hello hello példányok száma |
| subscriptionId |Igen |Hello cél erőforrás méretezett alatt álló előfizetés-azonosító |
| erőforráscsoport-név |Igen |Erőforráscsoport neve hello célerőforrása méretezett folyamatban |
| resourceName |Igen |Folyamatban méretezett hello cél erőforrás neve |
| a resourceType |Igen |hello támogatott értékek: "microsoft.classiccompute/domainnames/slots/roles" - szerepköröket, a "microsoft.compute/virtualmachinescalesets" - virtuálisgép-méretezési csoportok, és a "Microsoft.Web/serverfarms" - webalkalmazás felhőalapú szolgáltatás |
| resourceId |Igen |Hello cél erőforrás alatt méretezett erőforrás-kezelő azonosítója |
| portalLink |Igen |Az Azure portál hivatkozás toohello összegzés lapján hello célerőforrása |
| oldCapacity |Igen |hello aktuális (régi) példányszám amikor automatikus skálázás tartott tartozó skálázási műveletek |
| newCapacity |Igen |Hello, hogy automatikus skálázás hello erőforrás méretezve, túl új példányok száma|
| Tulajdonságok |Nem |Választható. < Kulcs értéke > Set párok (például szótár < String, String >). hello tulajdonságok mező kitöltése nem kötelező. Egy egyéni felhasználói felület vagy a Logic app alapú munkafolyamat, a kulcsok és értékek, amelyek átadhatók hello hasznos használatával is megadhatja. Egyéni tulajdonságok toopass biztonsági toohello kimenő webhook hívás más módja toouse hello webhook URI-JÁT magát (lekérdezési paraméterek) |
