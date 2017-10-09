---
title: "aaaAdvanced használata az Azure virtuális gépek automatikus skálázás |} Microsoft Docs"
description: "Erőforrás-kezelő és a Virtuálisgép-méretezési készlet használ, több szabályok és profilok, amely e-mailek küldése, és hívja meg a webhook URL-címet a skálázási művelet."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 7e3576e2-4a2b-4736-b5ae-98c4689cdd2b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2016
ms.author: ancav
ms.openlocfilehash: ecb01e3094f715837b75ef07a7dbecdf0f2e78f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a>Speciális automatikus skálázás konfigurációját a Resource Manager-sablonok segítségével a Virtuálisgép-méretezési készlet
Méretezés és a virtuálisgép-méretezési csoportok alapján a teljesítmény-küszöbértékeinek metrika, ismétlődő ütemezés szerint, vagy egy adott dátumot kibővített is. A skálázási műveletek értesítések e-mailek és a webhook is konfigurálhatja. Ez a bemutató ismerteti egy példa a Resource Manager-sablon használatával egy Virtuálisgép-méretezési csoportban lévő összes objektum konfigurálására.

> [!NOTE]
> Amíg ez a forgatókönyv bemutatja, hogyan hello a Virtuálisgép-méretezési készlet, hello ugyanazokat az információkat vonatkozik tooautoscaling [Felhőszolgáltatások](https://azure.microsoft.com/services/cloud-services/), és [App Service - webalkalmazások](https://azure.microsoft.com/services/app-service/web/).
> Egy egyszerű teljesítmény metrika például CPU-alapú egy egyszerű méretezési be- / kimeneti beállítást egy Virtuálisgép-méretezési csoportban, tekintse meg a toohello [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) és [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) dokumentumok
>
>

## <a name="walkthrough"></a>Útmutatás
Ebben a bemutatóban használjuk [Azure erőforrás-kezelő](https://resources.azure.com/) tooconfigure és frissítés hello automatikus skálázási beállítás skálázási készletének. Az Azure erőforrás-kezelő egy egyszerűen toomanage van Azure-erőforrások Resource Manager-sablonok használatával. Ha új tooAzure erőforrás-kezelő eszköz, olvassa el a [a bevezetés](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).

1. Telepítsen egy új méretezési készletben egy alapszintű automatikus skálázási beállítás. Ebben a cikkben az hello Azure gyorsindítási galéria, amely a Windows hello a méretezési alapvető automatikus skálázás sablonnal. Linux-skálázási készletekben munkahelyi hello azonos módon.
2. Hello méretezési csoport létrehozása után nyissa meg a toohello méretezési készlet erőforrás az Azure erőforrás-kezelővel. Hello következő Microsoft.Insights csomópont alatt megjelenik.

    ![Az Azure Explorer](./media/insights-advanced-autoscale-vmss/azure_explorer_navigate.png)

    hello sablon végrehajtási hozott létre egy alapértelmezett automatikus skálázási beállítás hello nevű **"autoscalewad"**. Hello jobb oldalán megtekintheti az automatikus skálázási beállítás hello teljes meghatározása. Ebben az esetben hello alapértelmezett automatikus skálázási beállítás CPU-alapú % kibővített és skálázási szabály tartalmaz.  

3. Hozzáadhat további profilok és hello ütemezés vagy a meghatározott követelmények alapján. Az automatikus skálázási beállítás nem létrehozni három profilok. toounderstand profilok és szabályok az automatikus skálázás, tekintse át a [automatikus skálázás gyakorlati tanácsok](insights-autoscale-best-practices.md).  

    | Profilok és szabályok | Leírás |
    |--- | --- |
    | **Profil** |**Teljesítmény/metrika alapján** |
    | Szabály |Service Bus várólista-üzenetek száma > x |
    | Szabály |Service Bus várólista-üzenetek száma < y |
    | Szabály |CPU % > n |
    | Szabály |CPU % < p |
    | **Profil** |**Milyen napra esik reggel óra (nincs szabály)** |
    | **Profil** |**A termék indítási nap (nincs szabály)** |

4. Íme egy elméleti méretezési forgatókönyv, hogy ez az útmutató használjuk.

    * **A terhelés alapján** -szeretném tooscale fel- vagy a skála set.* tárolt saját-alkalmazás hello terhelése alapján
    * **Üzenet-várólista mérete** -hello bejövő üzenetek toomy alkalmazás használatával a Service Bus-üzenetsorba. I hello várólista-üzenetek száma és a CPU %, és egy alapértelmezett profilt tootrigger tartozó skálázási műveletek konfigurálása, ha vagy üzenetek száma, vagy a CPU találatok hello threshold.*
    * **Heti és napi** -szeretném "Hétköznap reggel Hours" nevű hetente ismétlődő "idő hello nap" alapú profil. A korábbi adatok alapján, tudható, hogy bizonyos száma a Virtuálisgép-példányok toohandle az alkalmazás betöltése során a time.* jobb toohave
    * **Adott dátumok** -hozzáadott "Termék indítása Day" profil. I tervezzen előre az adott dátumok így egy alkalmazás készen áll a toohandle hello terhelés miatt marketing közlemények és amikor azt egy új terméken be hello application.*
    * *utolsó két hello-profil is más metrika alapján teljesítményszabályok rajtuk. Ebben az esetben I úgy döntött, hogy nem egy toohave, és helyette a toorely hello alapértelmezett teljesítmény metrika a szabályok alapján. Szabályok megadása nem kötelező hello ismétlődő és a dátum-alapú profilokhoz.*

    Hello-profilok és a szabályok automatikus skálázás motor rangsorolási is bekerül az hello [automatikus skálázás gyakorlati tanácsok](insights-autoscale-best-practices.md) cikk.
    Az automatikus skálázás közös metrikáját listája, [közös metrikáját automatikus skálázás](insights-autoscale-common-metrics.md)

5. Győződjön meg arról, hogy a hello **olvasási/írási** mód az erőforrás-kezelőben

    ![Autoscalewad, alapértelmezett automatikus skálázási beállítás](./media/insights-advanced-autoscale-vmss/autoscalewad.png)

6. Kattintson a Szerkesztés gombra. **Cserélje le** hello "profilok" elem a következő konfigurációs hello az automatikus skálázási beállításhoz:

    ![Profilok](./media/insights-advanced-autoscale-vmss/profiles.png)

    ```
    {
            "name": "Perf_Based_Scale",
            "capacity": {
              "minimum": "2",
              "maximum": "12",
              "default": "2"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 10
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 3
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 85
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 60
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          },
          {
            "name": "Weekday_Morning_Hours_Scale",
            "capacity": {
              "minimum": "4",
              "maximum": "12",
              "default": "4"
            },
            "rules": [],
            "recurrence": {
              "frequency": "Week",
              "schedule": {
                "timeZone": "Pacific Standard Time",
                "days": [
                  "Monday",
                  "Tuesday",
                  "Wednesday",
                  "Thursday",
                  "Friday"
                ],
                "hours": [
                  6
                ],
                "minutes": [
                  0
                ]
              }
            }
          },
          {
            "name": "Product_Launch_Day",
            "capacity": {
              "minimum": "6",
              "maximum": "20",
              "default": "6"
            },
            "rules": [],
            "fixedDate": {
              "timeZone": "Pacific Standard Time",
              "start": "2016-06-20T00:06:00Z",
              "end": "2016-06-21T23:59:00Z"
            }
          }
    ```
    Támogatott mezőket és azok értékeit: [automatikus skálázás REST API-dokumentáció](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx). Az automatikus skálázási beállítás már korábban ismertetett három profil hello tartalmazza.

7. Végül, nézze meg hello automatikus skálázás **értesítési** szakasz. Automatikus skálázás értesítések lehetővé teszik toodo három dolog, ha egy kibővített, vagy a művelet sikeresen elindítva.
   - Üdvözöljük a rendszergazdákat és az előfizetés társadminisztrátoroknak értesítése
   - E-mail-felhasználók
   - Indítás, webhook hívásakor. Következik be, amikor a webhook elküldi a metaadatokat hello automatikus skálázás feltételről és erőforrás-hello méretezési csoportban. További információ az automatikus skálázás webhook hello hasznos toolearn lásd [konfigurálja Webhook & automatikus skálázási az értesítő e-mailek](insights-autoscale-to-webhook-email.md).

   Adja hozzá a következő toohello automatikus skálázási beállítás cseréje hello a **értesítési** elem, amelynek az értéke null

   ```
   "notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": true,
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

   Találati **Put** erőforrás-kezelő tooupdate hello automatikus skálázási beállítás gombra.

Az automatikus skálázás beállítása egy Virtuálisgép-méretezési készlet tooinclude több skálázási profil frissítése befejeződött, és értesítéseket méretezni.

## <a name="next-steps"></a>Következő lépések
Használja a hivatkozások toolearn további információk az automatikus skálázást.

[Virtuálisgép-méretezési csoportok automatikusan skálázva hibaelhárítása](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[Az automatikus skálázás közös metrikák](insights-autoscale-common-metrics.md)

[Ajánlott eljárások az Azure automatikus skálázás](insights-autoscale-best-practices.md)

[Kezelheti az automatikus skálázás a PowerShell használatával](insights-powershell-samples.md#create-and-manage-autoscale-settings)

[Parancssori felület használatával automatikus skálázás kezelése](insights-cli-samples.md#autoscale)

[Webhook & értesítő e-mailek az automatikus skálázás konfigurálása](insights-autoscale-to-webhook-email.md)
