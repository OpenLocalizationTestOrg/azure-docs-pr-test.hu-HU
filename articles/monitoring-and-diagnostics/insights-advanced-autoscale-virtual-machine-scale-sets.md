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
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a><span data-ttu-id="f1dcd-103">Speciális automatikus skálázás konfigurációját a Resource Manager-sablonok segítségével a Virtuálisgép-méretezési készlet</span><span class="sxs-lookup"><span data-stu-id="f1dcd-103">Advanced autoscale configuration using Resource Manager templates for VM Scale Sets</span></span>
<span data-ttu-id="f1dcd-104">Méretezés és a virtuálisgép-méretezési csoportok alapján a teljesítmény-küszöbértékeinek metrika, ismétlődő ütemezés szerint, vagy egy adott dátumot kibővített is.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-104">You can scale-in and scale-out in Virtual Machine Scale Sets based on performance metric thresholds, by a recurring schedule, or by a particular date.</span></span> <span data-ttu-id="f1dcd-105">A skálázási műveletek értesítések e-mailek és a webhook is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-105">You can also configure email and webhook notifications for scale actions.</span></span> <span data-ttu-id="f1dcd-106">Ez a bemutató ismerteti egy példa a Resource Manager-sablon használatával egy Virtuálisgép-méretezési csoportban lévő összes objektum konfigurálására.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-106">This walkthrough shows an example of configuring all these objects using a Resource Manager template on a VM Scale Set.</span></span>

> [!NOTE]
> <span data-ttu-id="f1dcd-107">Amíg ez a forgatókönyv bemutatja, hogyan hello a Virtuálisgép-méretezési készlet, hello ugyanazokat az információkat vonatkozik tooautoscaling [Felhőszolgáltatások](https://azure.microsoft.com/services/cloud-services/), és [App Service - webalkalmazások](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="f1dcd-107">While this walkthrough explains hello steps for VM Scale Sets, hello same information applies tooautoscaling [Cloud Services](https://azure.microsoft.com/services/cloud-services/), and [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/).</span></span>
> <span data-ttu-id="f1dcd-108">Egy egyszerű teljesítmény metrika például CPU-alapú egy egyszerű méretezési be- / kimeneti beállítást egy Virtuálisgép-méretezési csoportban, tekintse meg a toohello [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) és [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) dokumentumok</span><span class="sxs-lookup"><span data-stu-id="f1dcd-108">For a simple scale in/out setting on a VM Scale Set based on a simple performance metric such as CPU, refer toohello [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) and [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) documents</span></span>
>
>

## <a name="walkthrough"></a><span data-ttu-id="f1dcd-109">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="f1dcd-109">Walkthrough</span></span>
<span data-ttu-id="f1dcd-110">Ebben a bemutatóban használjuk [Azure erőforrás-kezelő](https://resources.azure.com/) tooconfigure és frissítés hello automatikus skálázási beállítás skálázási készletének.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-110">In this walkthrough, we use [Azure Resource Explorer](https://resources.azure.com/) tooconfigure and update hello autoscale setting for a scale set.</span></span> <span data-ttu-id="f1dcd-111">Az Azure erőforrás-kezelő egy egyszerűen toomanage van Azure-erőforrások Resource Manager-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-111">Azure Resource Explorer is an easy way toomanage Azure resources via Resource Manager templates.</span></span> <span data-ttu-id="f1dcd-112">Ha új tooAzure erőforrás-kezelő eszköz, olvassa el a [a bevezetés](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).</span><span class="sxs-lookup"><span data-stu-id="f1dcd-112">If you are new tooAzure Resource Explorer tool, read [this introduction](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).</span></span>

1. <span data-ttu-id="f1dcd-113">Telepítsen egy új méretezési készletben egy alapszintű automatikus skálázási beállítás.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-113">Deploy a new scale set with a basic autoscale setting.</span></span> <span data-ttu-id="f1dcd-114">Ebben a cikkben az hello Azure gyorsindítási galéria, amely a Windows hello a méretezési alapvető automatikus skálázás sablonnal.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-114">This article uses hello one from hello Azure QuickStart Gallery, which has a Windows scale set with a basic autoscale template.</span></span> <span data-ttu-id="f1dcd-115">Linux-skálázási készletekben munkahelyi hello azonos módon.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-115">Linux scale sets work hello same way.</span></span>
2. <span data-ttu-id="f1dcd-116">Hello méretezési csoport létrehozása után nyissa meg a toohello méretezési készlet erőforrás az Azure erőforrás-kezelővel.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-116">After hello scale set is created, navigate toohello scale set resource from Azure Resource Explorer.</span></span> <span data-ttu-id="f1dcd-117">Hello következő Microsoft.Insights csomópont alatt megjelenik.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-117">You see hello following under Microsoft.Insights node.</span></span>

    ![Az Azure Explorer](./media/insights-advanced-autoscale-vmss/azure_explorer_navigate.png)

    <span data-ttu-id="f1dcd-119">hello sablon végrehajtási hozott létre egy alapértelmezett automatikus skálázási beállítás hello nevű **"autoscalewad"**.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-119">hello template execution has created a default autoscale setting with hello name **'autoscalewad'**.</span></span> <span data-ttu-id="f1dcd-120">Hello jobb oldalán megtekintheti az automatikus skálázási beállítás hello teljes meghatározása.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-120">On hello right-hand side, you can view hello full definition of this autoscale setting.</span></span> <span data-ttu-id="f1dcd-121">Ebben az esetben hello alapértelmezett automatikus skálázási beállítás CPU-alapú % kibővített és skálázási szabály tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-121">In this case, hello default autoscale setting comes with a CPU% based scale-out and scale-in rule.</span></span>  

3. <span data-ttu-id="f1dcd-122">Hozzáadhat további profilok és hello ütemezés vagy a meghatározott követelmények alapján.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-122">You can now add more profiles and rules based on hello schedule or specific requirements.</span></span> <span data-ttu-id="f1dcd-123">Az automatikus skálázási beállítás nem létrehozni három profilok.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-123">We create an autoscale setting with three profiles.</span></span> <span data-ttu-id="f1dcd-124">toounderstand profilok és szabályok az automatikus skálázás, tekintse át a [automatikus skálázás gyakorlati tanácsok](insights-autoscale-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="f1dcd-124">toounderstand profiles and rules in autoscale, review [Autoscale Best Practices](insights-autoscale-best-practices.md).</span></span>  

    | <span data-ttu-id="f1dcd-125">Profilok és szabályok</span><span class="sxs-lookup"><span data-stu-id="f1dcd-125">Profiles & Rules</span></span> | <span data-ttu-id="f1dcd-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="f1dcd-126">Description</span></span> |
    |--- | --- |
    | <span data-ttu-id="f1dcd-127">**Profil**</span><span class="sxs-lookup"><span data-stu-id="f1dcd-127">**Profile**</span></span> |<span data-ttu-id="f1dcd-128">**Teljesítmény/metrika alapján**</span><span class="sxs-lookup"><span data-stu-id="f1dcd-128">**Performance/metric based**</span></span> |
    | <span data-ttu-id="f1dcd-129">Szabály</span><span class="sxs-lookup"><span data-stu-id="f1dcd-129">Rule</span></span> |<span data-ttu-id="f1dcd-130">Service Bus várólista-üzenetek száma > x</span><span class="sxs-lookup"><span data-stu-id="f1dcd-130">Service Bus Queue Message Count > x</span></span> |
    | <span data-ttu-id="f1dcd-131">Szabály</span><span class="sxs-lookup"><span data-stu-id="f1dcd-131">Rule</span></span> |<span data-ttu-id="f1dcd-132">Service Bus várólista-üzenetek száma < y</span><span class="sxs-lookup"><span data-stu-id="f1dcd-132">Service Bus Queue Message Count < y</span></span> |
    | <span data-ttu-id="f1dcd-133">Szabály</span><span class="sxs-lookup"><span data-stu-id="f1dcd-133">Rule</span></span> |<span data-ttu-id="f1dcd-134">CPU % > n</span><span class="sxs-lookup"><span data-stu-id="f1dcd-134">CPU% > n</span></span> |
    | <span data-ttu-id="f1dcd-135">Szabály</span><span class="sxs-lookup"><span data-stu-id="f1dcd-135">Rule</span></span> |<span data-ttu-id="f1dcd-136">CPU % < p</span><span class="sxs-lookup"><span data-stu-id="f1dcd-136">CPU% < p</span></span> |
    | <span data-ttu-id="f1dcd-137">**Profil**</span><span class="sxs-lookup"><span data-stu-id="f1dcd-137">**Profile**</span></span> |<span data-ttu-id="f1dcd-138">**Milyen napra esik reggel óra (nincs szabály)**</span><span class="sxs-lookup"><span data-stu-id="f1dcd-138">**Weekday morning hours (no rules)**</span></span> |
    | <span data-ttu-id="f1dcd-139">**Profil**</span><span class="sxs-lookup"><span data-stu-id="f1dcd-139">**Profile**</span></span> |<span data-ttu-id="f1dcd-140">**A termék indítási nap (nincs szabály)**</span><span class="sxs-lookup"><span data-stu-id="f1dcd-140">**Product Launch day (no rules)**</span></span> |

4. <span data-ttu-id="f1dcd-141">Íme egy elméleti méretezési forgatókönyv, hogy ez az útmutató használjuk.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-141">Here is a hypothetical scaling scenario that we use for this walk-through.</span></span>

    * <span data-ttu-id="f1dcd-142">**A terhelés alapján** -szeretném tooscale fel- vagy a skála set.* tárolt saját-alkalmazás hello terhelése alapján</span><span class="sxs-lookup"><span data-stu-id="f1dcd-142">**Load based** - I'd like tooscale out or in based on hello load on my application hosted on my scale set.*</span></span>
    * <span data-ttu-id="f1dcd-143">**Üzenet-várólista mérete** -hello bejövő üzenetek toomy alkalmazás használatával a Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-143">**Message Queue size** - I use a Service Bus Queue for hello incoming messages toomy application.</span></span> <span data-ttu-id="f1dcd-144">I hello várólista-üzenetek száma és a CPU %, és egy alapértelmezett profilt tootrigger tartozó skálázási műveletek konfigurálása, ha vagy üzenetek száma, vagy a CPU találatok hello threshold.*</span><span class="sxs-lookup"><span data-stu-id="f1dcd-144">I use hello queue's message count and CPU% and configure a default profile tootrigger a scale action if either of message count or CPU hits hello threshold.*</span></span>
    * <span data-ttu-id="f1dcd-145">**Heti és napi** -szeretném "Hétköznap reggel Hours" nevű hetente ismétlődő "idő hello nap" alapú profil.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-145">**Time of week and day** - I want a weekly recurring 'time of hello day' based profile called 'Weekday Morning Hours'.</span></span> <span data-ttu-id="f1dcd-146">A korábbi adatok alapján, tudható, hogy bizonyos száma a Virtuálisgép-példányok toohandle az alkalmazás betöltése során a time.* jobb toohave</span><span class="sxs-lookup"><span data-stu-id="f1dcd-146">Based on historical data, I know it is better toohave certain number of VM instances toohandle my application's load during this time.*</span></span>
    * <span data-ttu-id="f1dcd-147">**Adott dátumok** -hozzáadott "Termék indítása Day" profil.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-147">**Special Dates** - I added a 'Product Launch Day' profile.</span></span> <span data-ttu-id="f1dcd-148">I tervezzen előre az adott dátumok így egy alkalmazás készen áll a toohandle hello terhelés miatt marketing közlemények és amikor azt egy új terméken be hello application.*</span><span class="sxs-lookup"><span data-stu-id="f1dcd-148">I plan ahead for specific dates so my application is ready toohandle hello load due marketing announcements and when we put a new product in hello application.*</span></span>
    * <span data-ttu-id="f1dcd-149">*utolsó két hello-profil is más metrika alapján teljesítményszabályok rajtuk. Ebben az esetben I úgy döntött, hogy nem egy toohave, és helyette a toorely hello alapértelmezett teljesítmény metrika a szabályok alapján. Szabályok megadása nem kötelező hello ismétlődő és a dátum-alapú profilokhoz.*</span><span class="sxs-lookup"><span data-stu-id="f1dcd-149">*hello last two profiles can also have other performance metric based rules within them. In this case, I decided not toohave one and instead toorely on hello default performance metric based rules. Rules are optional for hello recurring and date-based profiles.*</span></span>

    <span data-ttu-id="f1dcd-150">Hello-profilok és a szabályok automatikus skálázás motor rangsorolási is bekerül az hello [automatikus skálázás gyakorlati tanácsok](insights-autoscale-best-practices.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-150">Autoscale engine's prioritization of hello profiles and rules is also captured in hello [autoscaling best practices](insights-autoscale-best-practices.md) article.</span></span>
    <span data-ttu-id="f1dcd-151">Az automatikus skálázás közös metrikáját listája, [közös metrikáját automatikus skálázás](insights-autoscale-common-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="f1dcd-151">For a list of common metrics for autoscale, refer [Common metrics for Autoscale](insights-autoscale-common-metrics.md)</span></span>

5. <span data-ttu-id="f1dcd-152">Győződjön meg arról, hogy a hello **olvasási/írási** mód az erőforrás-kezelőben</span><span class="sxs-lookup"><span data-stu-id="f1dcd-152">Make sure you are on hello **Read/Write** mode in Resource Explorer</span></span>

    ![Autoscalewad, alapértelmezett automatikus skálázási beállítás](./media/insights-advanced-autoscale-vmss/autoscalewad.png)

6. <span data-ttu-id="f1dcd-154">Kattintson a Szerkesztés gombra.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-154">Click Edit.</span></span> <span data-ttu-id="f1dcd-155">**Cserélje le** hello "profilok" elem a következő konfigurációs hello az automatikus skálázási beállításhoz:</span><span class="sxs-lookup"><span data-stu-id="f1dcd-155">**Replace** hello 'profiles' element in autoscale setting with hello following configuration:</span></span>

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
    <span data-ttu-id="f1dcd-157">Támogatott mezőket és azok értékeit: [automatikus skálázás REST API-dokumentáció](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1dcd-157">For supported fields and their values, see [Autoscale REST API documentation](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx).</span></span> <span data-ttu-id="f1dcd-158">Az automatikus skálázási beállítás már korábban ismertetett három profil hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-158">Now your autoscale setting contains hello three profiles explained previously.</span></span>

7. <span data-ttu-id="f1dcd-159">Végül, nézze meg hello automatikus skálázás **értesítési** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-159">Finally, look at hello Autoscale **notification** section.</span></span> <span data-ttu-id="f1dcd-160">Automatikus skálázás értesítések lehetővé teszik toodo három dolog, ha egy kibővített, vagy a művelet sikeresen elindítva.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-160">Autoscale notifications allow you toodo three things when a scale-out or in action is successfully triggered.</span></span>
   - <span data-ttu-id="f1dcd-161">Üdvözöljük a rendszergazdákat és az előfizetés társadminisztrátoroknak értesítése</span><span class="sxs-lookup"><span data-stu-id="f1dcd-161">Notify hello admin and co-admins of your subscription</span></span>
   - <span data-ttu-id="f1dcd-162">E-mail-felhasználók</span><span class="sxs-lookup"><span data-stu-id="f1dcd-162">Email a set of users</span></span>
   - <span data-ttu-id="f1dcd-163">Indítás, webhook hívásakor.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-163">Trigger a webhook call.</span></span> <span data-ttu-id="f1dcd-164">Következik be, amikor a webhook elküldi a metaadatokat hello automatikus skálázás feltételről és erőforrás-hello méretezési csoportban.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-164">When fired, this webhook sends metadata about hello autoscaling condition and hello scale set resource.</span></span> <span data-ttu-id="f1dcd-165">További információ az automatikus skálázás webhook hello hasznos toolearn lásd [konfigurálja Webhook & automatikus skálázási az értesítő e-mailek](insights-autoscale-to-webhook-email.md).</span><span class="sxs-lookup"><span data-stu-id="f1dcd-165">toolearn more about hello payload of autoscale webhook, see [Configure Webhook & Email Notifications for Autoscale](insights-autoscale-to-webhook-email.md).</span></span>

   <span data-ttu-id="f1dcd-166">Adja hozzá a következő toohello automatikus skálázási beállítás cseréje hello a **értesítési** elem, amelynek az értéke null</span><span class="sxs-lookup"><span data-stu-id="f1dcd-166">Add hello following toohello Autoscale setting replacing your **notification** element whose value is null</span></span>

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

   <span data-ttu-id="f1dcd-167">Találati **Put** erőforrás-kezelő tooupdate hello automatikus skálázási beállítás gombra.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-167">Hit **Put** button in Resource Explorer tooupdate hello autoscale setting.</span></span>

<span data-ttu-id="f1dcd-168">Az automatikus skálázás beállítása egy Virtuálisgép-méretezési készlet tooinclude több skálázási profil frissítése befejeződött, és értesítéseket méretezni.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-168">You have updated an autoscale setting on a VM Scale set tooinclude multiple scale profiles and scale notifications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1dcd-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1dcd-169">Next Steps</span></span>
<span data-ttu-id="f1dcd-170">Használja a hivatkozások toolearn további információk az automatikus skálázást.</span><span class="sxs-lookup"><span data-stu-id="f1dcd-170">Use these links toolearn more about autoscaling.</span></span>

[<span data-ttu-id="f1dcd-171">Virtuálisgép-méretezési csoportok automatikusan skálázva hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="f1dcd-171">TroubleShoot Autoscale with Virtual Machine Scale Sets</span></span>](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[<span data-ttu-id="f1dcd-172">Az automatikus skálázás közös metrikák</span><span class="sxs-lookup"><span data-stu-id="f1dcd-172">Common Metrics for Autoscale</span></span>](insights-autoscale-common-metrics.md)

[<span data-ttu-id="f1dcd-173">Ajánlott eljárások az Azure automatikus skálázás</span><span class="sxs-lookup"><span data-stu-id="f1dcd-173">Best Practices for Azure Autoscale</span></span>](insights-autoscale-best-practices.md)

[<span data-ttu-id="f1dcd-174">Kezelheti az automatikus skálázás a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="f1dcd-174">Manage Autoscale using PowerShell</span></span>](insights-powershell-samples.md#create-and-manage-autoscale-settings)

[<span data-ttu-id="f1dcd-175">Parancssori felület használatával automatikus skálázás kezelése</span><span class="sxs-lookup"><span data-stu-id="f1dcd-175">Manage Autoscale using CLI</span></span>](insights-cli-samples.md#autoscale)

[<span data-ttu-id="f1dcd-176">Webhook & értesítő e-mailek az automatikus skálázás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1dcd-176">Configure Webhook & Email Notifications for Autoscale</span></span>](insights-autoscale-to-webhook-email.md)
