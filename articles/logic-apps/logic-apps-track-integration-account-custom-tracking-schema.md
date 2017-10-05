---
title: "Egyéni követési sémák B2B figyelés - Azure Logic Apps |} Microsoft Docs"
description: "Hozzon létre egyéni követési sémák figyelése az Azure-integráció fiókban tranzakciók B2B üzeneteit."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b71a4938dde2a71f1ce29403af7aa9101358d64c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-tracking-to-monitor-your-complete-workflow-end-to-end"></a><span data-ttu-id="7d5c4-103">A teljes, végpontok közötti munkafolyamat figyelése nyomon követése</span><span class="sxs-lookup"><span data-stu-id="7d5c4-103">Enable tracking to monitor your complete workflow, end-to-end</span></span>
<span data-ttu-id="7d5c4-104">Nincs beépített nyomon követése, hogy engedélyezheti a vállalatok munkafolyamat, például követési AS2 vagy X12 egyes részeinek üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-104">There is built-in tracking that you can enable for different parts of your business-to-business workflow, such as tracking AS2 or X12 messages.</span></span> <span data-ttu-id="7d5c4-105">Logikai alkalmazás, a BizTalk Serveren, SQL Server vagy más réteg munkafolyamatok létrehozásakor tartalmazza, majd engedélyezheti egyéni, amely eseményeket naplózza az elejéről, a munkafolyamat végén.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-105">When you create workflows that includes a logic app, BizTalk Server, SQL Server, or any other layer, then you can enable custom tracking that logs events from the beginning to the end of your workflow.</span></span> 

<span data-ttu-id="7d5c4-106">Ez a témakör a rétegek kívül a logikai alkalmazás használhat egyéni kódot.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-106">This topic provides custom code that you can use in the layers outside of your logic app.</span></span> 

## <a name="custom-tracking-schema"></a><span data-ttu-id="7d5c4-107">Egyéni követési séma</span><span class="sxs-lookup"><span data-stu-id="7d5c4-107">Custom tracking schema</span></span>
````java

        {
            "sourceType": "",
            "source": {

            "workflow": {
                "systemId": ""
            },
            "runInstance": {
                "runId": ""
            },
            "operation": {
                "operationName": "",
                "repeatItemScopeName": "",
                "repeatItemIndex": "",
                "trackingId": "",
                "correlationId": "",
                "clientRequestId": ""
                }
            },
            "events": [
            {
                "eventLevel": "",
                "eventTime": "",
                "recordType": "",
                "record": {                
                }
            }
         ]
      }

````

| <span data-ttu-id="7d5c4-108">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7d5c4-108">Property</span></span> | <span data-ttu-id="7d5c4-109">Típus</span><span class="sxs-lookup"><span data-stu-id="7d5c4-109">Type</span></span> | <span data-ttu-id="7d5c4-110">Leírás</span><span class="sxs-lookup"><span data-stu-id="7d5c4-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7d5c4-111">Forrástípus</span><span class="sxs-lookup"><span data-stu-id="7d5c4-111">sourceType</span></span> |   | <span data-ttu-id="7d5c4-112">A futtatási forrás típusa.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-112">Type of the run source.</span></span> <span data-ttu-id="7d5c4-113">Két érték engedélyezett **Microsoft.Logic/workflows** és **egyéni**.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-113">Allowed values are **Microsoft.Logic/workflows** and **custom**.</span></span> <span data-ttu-id="7d5c4-114">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-114">(Mandatory)</span></span> |
| <span data-ttu-id="7d5c4-115">Forrás</span><span class="sxs-lookup"><span data-stu-id="7d5c4-115">Source</span></span> |   | <span data-ttu-id="7d5c4-116">Ha a forrás típusa **Microsoft.Logic/workflows**, kövesse az ebben a sémában kell az adatforrásra vonatkozó információ.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-116">If the source type is **Microsoft.Logic/workflows**, the source information needs to follow this schema.</span></span> <span data-ttu-id="7d5c4-117">Ha a forrás típusa **egyéni**, a séma egy JToken.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-117">If the source type is **custom**, the schema is a JToken.</span></span> <span data-ttu-id="7d5c4-118">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-118">(Mandatory)</span></span> |
| <span data-ttu-id="7d5c4-119">Rendszerazonosító</span><span class="sxs-lookup"><span data-stu-id="7d5c4-119">systemId</span></span> | <span data-ttu-id="7d5c4-120">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d5c4-120">String</span></span> | <span data-ttu-id="7d5c4-121">Logic app rendszer azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-121">Logic app system ID.</span></span> <span data-ttu-id="7d5c4-122">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-122">(Mandatory)</span></span> |
| <span data-ttu-id="7d5c4-123">futtatásazonosító</span><span class="sxs-lookup"><span data-stu-id="7d5c4-123">runId</span></span> | <span data-ttu-id="7d5c4-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d5c4-124">String</span></span> | <span data-ttu-id="7d5c4-125">Logikai alkalmazás futtatásához azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-125">Logic app run ID.</span></span> <span data-ttu-id="7d5c4-126">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-126">(Mandatory)</span></span> |
| <span data-ttu-id="7d5c4-127">operationName</span><span class="sxs-lookup"><span data-stu-id="7d5c4-127">operationName</span></span> | <span data-ttu-id="7d5c4-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d5c4-128">String</span></span> | <span data-ttu-id="7d5c4-129">A művelet (például a művelet vagy az eseményindító) neve.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-129">Name of the operation (for example, action or trigger).</span></span> <span data-ttu-id="7d5c4-130">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-130">(Mandatory)</span></span> |
| <span data-ttu-id="7d5c4-131">repeatItemScopeName</span><span class="sxs-lookup"><span data-stu-id="7d5c4-131">repeatItemScopeName</span></span> | <span data-ttu-id="7d5c4-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d5c4-132">String</span></span> | <span data-ttu-id="7d5c4-133">Ismételje meg az elem neve, ha a művelet belül egy `foreach` / `until` hurok.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-133">Repeat item name if the action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="7d5c4-134">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-134">(Mandatory)</span></span> |
| <span data-ttu-id="7d5c4-135">repeatItemIndex</span><span class="sxs-lookup"><span data-stu-id="7d5c4-135">repeatItemIndex</span></span> | <span data-ttu-id="7d5c4-136">Egész szám</span><span class="sxs-lookup"><span data-stu-id="7d5c4-136">Integer</span></span> | <span data-ttu-id="7d5c4-137">Hogy a művelet belül van-e egy `foreach` / `until` hurok.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-137">Whether the action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="7d5c4-138">Azt jelzi, hogy az ismétlődő elemek index.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-138">Indicates the repeated item index.</span></span> <span data-ttu-id="7d5c4-139">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-139">(Mandatory)</span></span> |
| <span data-ttu-id="7d5c4-140">trackingId</span><span class="sxs-lookup"><span data-stu-id="7d5c4-140">trackingId</span></span> | <span data-ttu-id="7d5c4-141">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d5c4-141">String</span></span> | <span data-ttu-id="7d5c4-142">Nyomkövetési azonosító, az üzenetek összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-142">Tracking ID, to correlate the messages.</span></span> <span data-ttu-id="7d5c4-143">(Választható)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-143">(Optional)</span></span> |
| <span data-ttu-id="7d5c4-144">correlationId</span><span class="sxs-lookup"><span data-stu-id="7d5c4-144">correlationId</span></span> | <span data-ttu-id="7d5c4-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d5c4-145">String</span></span> | <span data-ttu-id="7d5c4-146">Korrelációs azonosító, az üzenetek összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-146">Correlation ID, to correlate the messages.</span></span> <span data-ttu-id="7d5c4-147">(Választható)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-147">(Optional)</span></span> |
| <span data-ttu-id="7d5c4-148">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="7d5c4-148">clientRequestId</span></span> | <span data-ttu-id="7d5c4-149">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d5c4-149">String</span></span> | <span data-ttu-id="7d5c4-150">Ügyfél töltheti fel az üzenetek összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-150">Client can populate it to correlate messages.</span></span> <span data-ttu-id="7d5c4-151">(Választható)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-151">(Optional)</span></span> |
| <span data-ttu-id="7d5c4-152">EventLevel</span><span class="sxs-lookup"><span data-stu-id="7d5c4-152">eventLevel</span></span> |   | <span data-ttu-id="7d5c4-153">Az esemény szintje.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-153">Level of the event.</span></span> <span data-ttu-id="7d5c4-154">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-154">(Mandatory)</span></span> |
| <span data-ttu-id="7d5c4-155">eventTime</span><span class="sxs-lookup"><span data-stu-id="7d5c4-155">eventTime</span></span> |   | <span data-ttu-id="7d5c4-156">ÉÉÉÉ-hh-DDTHH:MM:SS.00000Z UTC formátumban, az esemény időpontja.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-156">Time of the event, in UTC format YYYY-MM-DDTHH:MM:SS.00000Z.</span></span> <span data-ttu-id="7d5c4-157">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-157">(Mandatory)</span></span> |
| <span data-ttu-id="7d5c4-158">recordType</span><span class="sxs-lookup"><span data-stu-id="7d5c4-158">recordType</span></span> |   | <span data-ttu-id="7d5c4-159">A track rekord típusa.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-159">Type of the track record.</span></span> <span data-ttu-id="7d5c4-160">Az engedélyezett értéket **egyéni**.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-160">Allowed value is **custom**.</span></span> <span data-ttu-id="7d5c4-161">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-161">(Mandatory)</span></span> |
| <span data-ttu-id="7d5c4-162">rekord</span><span class="sxs-lookup"><span data-stu-id="7d5c4-162">record</span></span> |   | <span data-ttu-id="7d5c4-163">Egyéni rekordtípus.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-163">Custom record type.</span></span> <span data-ttu-id="7d5c4-164">Engedélyezett JToken érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="7d5c4-164">Allowed format is JToken.</span></span> <span data-ttu-id="7d5c4-165">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="7d5c4-165">(Mandatory)</span></span> |

## <a name="b2b-protocol-tracking-schemas"></a><span data-ttu-id="7d5c4-166">B2B protokoll követési sémák</span><span class="sxs-lookup"><span data-stu-id="7d5c4-166">B2B protocol tracking schemas</span></span>
<span data-ttu-id="7d5c4-167">Követés sémák B2B protokoll kapcsolatos információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="7d5c4-167">For information about B2B protocol tracking schemas, see:</span></span>
* [<span data-ttu-id="7d5c4-168">AS2-követési sémák</span><span class="sxs-lookup"><span data-stu-id="7d5c4-168">AS2 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [<span data-ttu-id="7d5c4-169">X12-követési sémák</span><span class="sxs-lookup"><span data-stu-id="7d5c4-169">X12 tracking schemas</span></span>](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a><span data-ttu-id="7d5c4-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7d5c4-170">Next steps</span></span>
* <span data-ttu-id="7d5c4-171">További információ [B2B üzenetek figyelése](logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="7d5c4-171">Learn more about [monitoring B2B messages](logic-apps-monitor-b2b-message.md).</span></span>   
* <span data-ttu-id="7d5c4-172">További tudnivalók [az Operations Management Suite-portálon B2B üzenetek nyomon követése](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="7d5c4-172">Learn about [tracking B2B messages in the Operations Management Suite portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>
* <span data-ttu-id="7d5c4-173">További információ a [vállalati integrációs csomag](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7d5c4-173">Learn more about the [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span>
