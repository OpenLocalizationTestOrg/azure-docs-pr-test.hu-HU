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
# <a name="use-autoscale-actions-toosend-email-and-webhook-alert-notifications-in-azure-monitor"></a><span data-ttu-id="d1720-104">Automatikus skálázási műveletek toosend e-mailek és a webhook riasztási értesítések használata az Azure-figyelő</span><span class="sxs-lookup"><span data-stu-id="d1720-104">Use autoscale actions toosend email and webhook alert notifications in Azure Monitor</span></span>
<span data-ttu-id="d1720-105">Ez a cikk bemutatja, hogyan lehet beállítani az eseményindítók, hogy az adott webes URL-címek hívja, vagy az Azure-ban automatikus skálázási műveletek alapján a e-mailek küldése.</span><span class="sxs-lookup"><span data-stu-id="d1720-105">This article shows you how set up triggers so that you can call specific web URLs or send emails based on autoscale actions in Azure.</span></span>  

## <a name="webhooks"></a><span data-ttu-id="d1720-106">webhook</span><span class="sxs-lookup"><span data-stu-id="d1720-106">Webhooks</span></span>
<span data-ttu-id="d1720-107">Webhook tooroute hello Azure riasztási értesítések tooother rendszerek utófeldolgozási vagy egyéni értesítések engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d1720-107">Webhooks allow you tooroute hello Azure alert notifications tooother systems for post-processing or custom notifications.</span></span> <span data-ttu-id="d1720-108">Például egy bejövő webes kérelem toosend SMS, napló hibák kezelhető útválasztási hello riasztási tooservices értesítés Csevegés használata vagy üzenetküldési szolgáltatások csoport, stb. hello webhook URI kell lennie egy érvényes HTTP vagy HTTPS-végponton.</span><span class="sxs-lookup"><span data-stu-id="d1720-108">For example, routing hello alert tooservices that can handle an incoming web request toosend SMS, log bugs, notify a team using chat or messaging services, etc. hello webhook URI must be a valid HTTP or HTTPS endpoint.</span></span>

## <a name="email"></a><span data-ttu-id="d1720-109">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="d1720-109">Email</span></span>
<span data-ttu-id="d1720-110">E-mail küldhető tooany érvényes e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="d1720-110">Email can be sent tooany valid email address.</span></span> <span data-ttu-id="d1720-111">A rendszergazdák és a társadminisztrátorok hello szabály futtató hello előfizetés is értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="d1720-111">Administrators and co-administrators of hello subscription where hello rule is running will also be notified.</span></span>

## <a name="cloud-services-and-web-apps"></a><span data-ttu-id="d1720-112">Cloud Services és a Web Apps</span><span class="sxs-lookup"><span data-stu-id="d1720-112">Cloud Services and Web Apps</span></span>
<span data-ttu-id="d1720-113">Ön részt hello Azure-portálon a felhőalapú szolgáltatásokhoz és a kiszolgálófarmok (webalkalmazások).</span><span class="sxs-lookup"><span data-stu-id="d1720-113">You can opt-in from hello Azure portal for Cloud Services and Server Farms (Web Apps).</span></span>

* <span data-ttu-id="d1720-114">Válassza ki a hello **szerint** metrikát.</span><span class="sxs-lookup"><span data-stu-id="d1720-114">Choose hello **scale by** metric.</span></span>

![Bővítse a](./media/insights-autoscale-to-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a><span data-ttu-id="d1720-116">Virtuálisgép-méretezési csoportok</span><span class="sxs-lookup"><span data-stu-id="d1720-116">Virtual Machine scale sets</span></span>
<span data-ttu-id="d1720-117">Újabb virtuális gépek létrehozása a Resource Manager (virtuálisgép-méretezési csoportok) konfigurálhatja a REST API-t, a Resource Manager sablonok, a PowerShell és a parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="d1720-117">For newer Virtual Machines created with Resource Manager (Virtual Machine scale sets), you can configure this using REST API, Resource Manager templates, PowerShell, and CLI.</span></span> <span data-ttu-id="d1720-118">A portál felület még nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="d1720-118">A portal interface is not yet available.</span></span>
<span data-ttu-id="d1720-119">Ha hello REST API-t vagy a Resource Manager-sablon, vegye fel a hello értesítések elem az alábbi beállítások hello.</span><span class="sxs-lookup"><span data-stu-id="d1720-119">When using hello REST API or Resource Manager template, include hello notifications element with hello following options.</span></span>

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
| <span data-ttu-id="d1720-120">Mező</span><span class="sxs-lookup"><span data-stu-id="d1720-120">Field</span></span> | <span data-ttu-id="d1720-121">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="d1720-121">Mandatory?</span></span> | <span data-ttu-id="d1720-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="d1720-122">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d1720-123">művelet</span><span class="sxs-lookup"><span data-stu-id="d1720-123">operation</span></span> |<span data-ttu-id="d1720-124">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-124">yes</span></span> |<span data-ttu-id="d1720-125">Az értéknek kell lennie a "Méretezési"</span><span class="sxs-lookup"><span data-stu-id="d1720-125">value must be "Scale"</span></span> |
| <span data-ttu-id="d1720-126">sendToSubscriptionAdministrator</span><span class="sxs-lookup"><span data-stu-id="d1720-126">sendToSubscriptionAdministrator</span></span> |<span data-ttu-id="d1720-127">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-127">yes</span></span> |<span data-ttu-id="d1720-128">érték lehet "true" vagy "false"</span><span class="sxs-lookup"><span data-stu-id="d1720-128">value must be "true" or "false"</span></span> |
| <span data-ttu-id="d1720-129">sendToSubscriptionCoAdministrators</span><span class="sxs-lookup"><span data-stu-id="d1720-129">sendToSubscriptionCoAdministrators</span></span> |<span data-ttu-id="d1720-130">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-130">yes</span></span> |<span data-ttu-id="d1720-131">érték lehet "true" vagy "false"</span><span class="sxs-lookup"><span data-stu-id="d1720-131">value must be "true" or "false"</span></span> |
| <span data-ttu-id="d1720-132">customEmails</span><span class="sxs-lookup"><span data-stu-id="d1720-132">customEmails</span></span> |<span data-ttu-id="d1720-133">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-133">yes</span></span> |<span data-ttu-id="d1720-134">érték lehet null értékű [] vagy e-mailek karakterlánc tömbje</span><span class="sxs-lookup"><span data-stu-id="d1720-134">value can be null [] or string array of emails</span></span> |
| <span data-ttu-id="d1720-135">webhook</span><span class="sxs-lookup"><span data-stu-id="d1720-135">webhooks</span></span> |<span data-ttu-id="d1720-136">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-136">yes</span></span> |<span data-ttu-id="d1720-137">értéke lehet null vagy érvénytelen Uri</span><span class="sxs-lookup"><span data-stu-id="d1720-137">value can be null or valid Uri</span></span> |
| <span data-ttu-id="d1720-138">serviceUri</span><span class="sxs-lookup"><span data-stu-id="d1720-138">serviceUri</span></span> |<span data-ttu-id="d1720-139">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-139">yes</span></span> |<span data-ttu-id="d1720-140">egy érvényes https Uri</span><span class="sxs-lookup"><span data-stu-id="d1720-140">a valid https Uri</span></span> |
| <span data-ttu-id="d1720-141">properties</span><span class="sxs-lookup"><span data-stu-id="d1720-141">properties</span></span> |<span data-ttu-id="d1720-142">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-142">yes</span></span> |<span data-ttu-id="d1720-143">érték üres {} kell lennie, vagy tartalmazhat kulcs-érték párok</span><span class="sxs-lookup"><span data-stu-id="d1720-143">value must be empty {} or can contain key-value pairs</span></span> |

## <a name="authentication-in-webhooks"></a><span data-ttu-id="d1720-144">A webhook hitelesítés</span><span class="sxs-lookup"><span data-stu-id="d1720-144">Authentication in webhooks</span></span>
<span data-ttu-id="d1720-145">hello webhook hitelesítheti a jogkivonat-alapú hitelesítés használatát, mentési helyét hello webhook URI lekérdezési paraméterként jogkivonat-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="d1720-145">hello webhook can authenticate using token-based authentication, where you save hello webhook URI with a token ID as a query parameter.</span></span> <span data-ttu-id="d1720-146">Például https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue</span><span class="sxs-lookup"><span data-stu-id="d1720-146">For example, https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue</span></span>

## <a name="autoscale-notification-webhook-payload-schema"></a><span data-ttu-id="d1720-147">Automatikus skálázás értesítési webhook hasznos séma</span><span class="sxs-lookup"><span data-stu-id="d1720-147">Autoscale notification webhook payload schema</span></span>
<span data-ttu-id="d1720-148">Hello automatikus skálázás értesítést generál, amikor hello következő metaadatokat tartalmazza hello webhook hasznos:</span><span class="sxs-lookup"><span data-stu-id="d1720-148">When hello autoscale notification is generated, hello following metadata is included in hello webhook payload:</span></span>

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


| <span data-ttu-id="d1720-149">Mező</span><span class="sxs-lookup"><span data-stu-id="d1720-149">Field</span></span> | <span data-ttu-id="d1720-150">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="d1720-150">Mandatory?</span></span> | <span data-ttu-id="d1720-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="d1720-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d1720-152">status</span><span class="sxs-lookup"><span data-stu-id="d1720-152">status</span></span> |<span data-ttu-id="d1720-153">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-153">yes</span></span> |<span data-ttu-id="d1720-154">hello állapot jelzi, hogy létrejött-e egy automatikus skálázási művelet</span><span class="sxs-lookup"><span data-stu-id="d1720-154">hello status that indicates that an autoscale action was generated</span></span> |
| <span data-ttu-id="d1720-155">művelet</span><span class="sxs-lookup"><span data-stu-id="d1720-155">operation</span></span> |<span data-ttu-id="d1720-156">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-156">yes</span></span> |<span data-ttu-id="d1720-157">A példányok növelését "Horizontális Felskálázás" lesz, és példányok csökkenése, lesz az "Méretezési a"</span><span class="sxs-lookup"><span data-stu-id="d1720-157">For an increase of instances, it will be "Scale Out" and for a decrease in instances, it will be "Scale In"</span></span> |
| <span data-ttu-id="d1720-158">A környezetben</span><span class="sxs-lookup"><span data-stu-id="d1720-158">context</span></span> |<span data-ttu-id="d1720-159">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-159">yes</span></span> |<span data-ttu-id="d1720-160">hello automatikus skálázási művelet környezetben</span><span class="sxs-lookup"><span data-stu-id="d1720-160">hello autoscale action context</span></span> |
| <span data-ttu-id="d1720-161">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="d1720-161">timestamp</span></span> |<span data-ttu-id="d1720-162">igen</span><span class="sxs-lookup"><span data-stu-id="d1720-162">yes</span></span> |<span data-ttu-id="d1720-163">Időbélyegzőt, ha hello automatikus skálázási művelet lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="d1720-163">Time stamp when hello autoscale action was triggered</span></span> |
| <span data-ttu-id="d1720-164">id</span><span class="sxs-lookup"><span data-stu-id="d1720-164">id</span></span> |<span data-ttu-id="d1720-165">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-165">Yes</span></span> |<span data-ttu-id="d1720-166">Erőforrás-kezelő azonosító hello automatikus skálázási beállítás</span><span class="sxs-lookup"><span data-stu-id="d1720-166">Resource Manager ID of hello autoscale setting</span></span> |
| <span data-ttu-id="d1720-167">név</span><span class="sxs-lookup"><span data-stu-id="d1720-167">name</span></span> |<span data-ttu-id="d1720-168">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-168">Yes</span></span> |<span data-ttu-id="d1720-169">hello hello automatikus skálázási beállítás neve</span><span class="sxs-lookup"><span data-stu-id="d1720-169">hello name of hello autoscale setting</span></span> |
| <span data-ttu-id="d1720-170">Részletek</span><span class="sxs-lookup"><span data-stu-id="d1720-170">details</span></span> |<span data-ttu-id="d1720-171">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-171">Yes</span></span> |<span data-ttu-id="d1720-172">Tartott magyarázatát, hogy az automatikus skálázás szolgáltatás hello hello műveletet, és a hello hello példányok száma</span><span class="sxs-lookup"><span data-stu-id="d1720-172">Explanation of hello action that hello autoscale service took and hello change in hello instance count</span></span> |
| <span data-ttu-id="d1720-173">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="d1720-173">subscriptionId</span></span> |<span data-ttu-id="d1720-174">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-174">Yes</span></span> |<span data-ttu-id="d1720-175">Hello cél erőforrás méretezett alatt álló előfizetés-azonosító</span><span class="sxs-lookup"><span data-stu-id="d1720-175">Subscription ID of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="d1720-176">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="d1720-176">resourceGroupName</span></span> |<span data-ttu-id="d1720-177">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-177">Yes</span></span> |<span data-ttu-id="d1720-178">Erőforráscsoport neve hello célerőforrása méretezett folyamatban</span><span class="sxs-lookup"><span data-stu-id="d1720-178">Resource Group name of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="d1720-179">resourceName</span><span class="sxs-lookup"><span data-stu-id="d1720-179">resourceName</span></span> |<span data-ttu-id="d1720-180">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-180">Yes</span></span> |<span data-ttu-id="d1720-181">Folyamatban méretezett hello cél erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="d1720-181">Name of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="d1720-182">a resourceType</span><span class="sxs-lookup"><span data-stu-id="d1720-182">resourceType</span></span> |<span data-ttu-id="d1720-183">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-183">Yes</span></span> |<span data-ttu-id="d1720-184">hello támogatott értékek: "microsoft.classiccompute/domainnames/slots/roles" - szerepköröket, a "microsoft.compute/virtualmachinescalesets" - virtuálisgép-méretezési csoportok, és a "Microsoft.Web/serverfarms" - webalkalmazás felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d1720-184">hello three supported values: "microsoft.classiccompute/domainnames/slots/roles" - Cloud Service roles, "microsoft.compute/virtualmachinescalesets" - Virtual Machine Scale Sets,  and "Microsoft.Web/serverfarms" - Web App</span></span> |
| <span data-ttu-id="d1720-185">resourceId</span><span class="sxs-lookup"><span data-stu-id="d1720-185">resourceId</span></span> |<span data-ttu-id="d1720-186">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-186">Yes</span></span> |<span data-ttu-id="d1720-187">Hello cél erőforrás alatt méretezett erőforrás-kezelő azonosítója</span><span class="sxs-lookup"><span data-stu-id="d1720-187">Resource Manager ID of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="d1720-188">portalLink</span><span class="sxs-lookup"><span data-stu-id="d1720-188">portalLink</span></span> |<span data-ttu-id="d1720-189">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-189">Yes</span></span> |<span data-ttu-id="d1720-190">Az Azure portál hivatkozás toohello összegzés lapján hello célerőforrása</span><span class="sxs-lookup"><span data-stu-id="d1720-190">Azure portal link toohello summary page of hello target resource</span></span> |
| <span data-ttu-id="d1720-191">oldCapacity</span><span class="sxs-lookup"><span data-stu-id="d1720-191">oldCapacity</span></span> |<span data-ttu-id="d1720-192">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-192">Yes</span></span> |<span data-ttu-id="d1720-193">hello aktuális (régi) példányszám amikor automatikus skálázás tartott tartozó skálázási műveletek</span><span class="sxs-lookup"><span data-stu-id="d1720-193">hello current (old) instance count when Autoscale took a scale action</span></span> |
| <span data-ttu-id="d1720-194">newCapacity</span><span class="sxs-lookup"><span data-stu-id="d1720-194">newCapacity</span></span> |<span data-ttu-id="d1720-195">Igen</span><span class="sxs-lookup"><span data-stu-id="d1720-195">Yes</span></span> |<span data-ttu-id="d1720-196">Hello, hogy automatikus skálázás hello erőforrás méretezve, túl új példányok száma</span><span class="sxs-lookup"><span data-stu-id="d1720-196">hello new instance count that Autoscale scaled hello resource too</span></span>|
| <span data-ttu-id="d1720-197">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="d1720-197">Properties</span></span> |<span data-ttu-id="d1720-198">Nem</span><span class="sxs-lookup"><span data-stu-id="d1720-198">No</span></span> |<span data-ttu-id="d1720-199">Választható.</span><span class="sxs-lookup"><span data-stu-id="d1720-199">Optional.</span></span> <span data-ttu-id="d1720-200">< Kulcs értéke > Set párok (például szótár < String, String >).</span><span class="sxs-lookup"><span data-stu-id="d1720-200">Set of <Key, Value> pairs (for example,  Dictionary <String, String>).</span></span> <span data-ttu-id="d1720-201">hello tulajdonságok mező kitöltése nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="d1720-201">hello properties field is optional.</span></span> <span data-ttu-id="d1720-202">Egy egyéni felhasználói felület vagy a Logic app alapú munkafolyamat, a kulcsok és értékek, amelyek átadhatók hello hasznos használatával is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="d1720-202">In a custom user interface  or Logic app based workflow, you can enter keys and values that can be passed using hello payload.</span></span> <span data-ttu-id="d1720-203">Egyéni tulajdonságok toopass biztonsági toohello kimenő webhook hívás más módja toouse hello webhook URI-JÁT magát (lekérdezési paraméterek)</span><span class="sxs-lookup"><span data-stu-id="d1720-203">An alternate way toopass custom properties back toohello outgoing webhook call is toouse hello webhook URI itself (as query parameters)</span></span> |
