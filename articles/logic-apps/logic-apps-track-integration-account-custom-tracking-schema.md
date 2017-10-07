---
title: "követés sémák B2B aaaCustom figyelési - Azure Logic Apps |} Microsoft Docs"
description: "Hozzon létre egyéni követési sémák toomonitor B2B üzenetek a tranzakciók az Azure-integráció fiókban."
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
ms.openlocfilehash: 8cf26a43d89f0414a2a8c5ef59d804235afeb5d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-tracking-toomonitor-your-complete-workflow-end-to-end"></a><span data-ttu-id="ee74b-103">Engedélyezi a nyomkövetési toomonitor munkafolyamat, végpontok közötti</span><span class="sxs-lookup"><span data-stu-id="ee74b-103">Enable tracking toomonitor your complete workflow, end-to-end</span></span>
<span data-ttu-id="ee74b-104">Nincs beépített nyomon követése, hogy engedélyezheti a vállalatok munkafolyamat, például követési AS2 vagy X12 egyes részeinek üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="ee74b-104">There is built-in tracking that you can enable for different parts of your business-to-business workflow, such as tracking AS2 or X12 messages.</span></span> <span data-ttu-id="ee74b-105">Amikor hoz létre, amely tartalmazza a logikai alkalmazás, a BizTalk Serveren, SQL Server vagy bármely más réteg munkafolyamatok, majd engedélyezheti egyéni, amely eseményeket naplózza a munkafolyamat a hello elejére toohello végére.</span><span class="sxs-lookup"><span data-stu-id="ee74b-105">When you create workflows that includes a logic app, BizTalk Server, SQL Server, or any other layer, then you can enable custom tracking that logs events from hello beginning toohello end of your workflow.</span></span> 

<span data-ttu-id="ee74b-106">Ez a témakör olyan egyéni kód, amely kívül a logikai alkalmazás hello rétegek használhatja.</span><span class="sxs-lookup"><span data-stu-id="ee74b-106">This topic provides custom code that you can use in hello layers outside of your logic app.</span></span> 

## <a name="custom-tracking-schema"></a><span data-ttu-id="ee74b-107">Egyéni követési séma</span><span class="sxs-lookup"><span data-stu-id="ee74b-107">Custom tracking schema</span></span>
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

| <span data-ttu-id="ee74b-108">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ee74b-108">Property</span></span> | <span data-ttu-id="ee74b-109">Típus</span><span class="sxs-lookup"><span data-stu-id="ee74b-109">Type</span></span> | <span data-ttu-id="ee74b-110">Leírás</span><span class="sxs-lookup"><span data-stu-id="ee74b-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee74b-111">Forrástípus</span><span class="sxs-lookup"><span data-stu-id="ee74b-111">sourceType</span></span> |   | <span data-ttu-id="ee74b-112">Futtatás hello forrás típusa.</span><span class="sxs-lookup"><span data-stu-id="ee74b-112">Type of hello run source.</span></span> <span data-ttu-id="ee74b-113">Két érték engedélyezett **Microsoft.Logic/workflows** és **egyéni**.</span><span class="sxs-lookup"><span data-stu-id="ee74b-113">Allowed values are **Microsoft.Logic/workflows** and **custom**.</span></span> <span data-ttu-id="ee74b-114">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-114">(Mandatory)</span></span> |
| <span data-ttu-id="ee74b-115">Forrás</span><span class="sxs-lookup"><span data-stu-id="ee74b-115">Source</span></span> |   | <span data-ttu-id="ee74b-116">Ha hello forrástípus **Microsoft.Logic/workflows**, hello adatforrásra vonatkozó információ kell toofollow ebben a sémában.</span><span class="sxs-lookup"><span data-stu-id="ee74b-116">If hello source type is **Microsoft.Logic/workflows**, hello source information needs toofollow this schema.</span></span> <span data-ttu-id="ee74b-117">Ha hello forrástípus **egyéni**, hello séma egy JToken.</span><span class="sxs-lookup"><span data-stu-id="ee74b-117">If hello source type is **custom**, hello schema is a JToken.</span></span> <span data-ttu-id="ee74b-118">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-118">(Mandatory)</span></span> |
| <span data-ttu-id="ee74b-119">Rendszerazonosító</span><span class="sxs-lookup"><span data-stu-id="ee74b-119">systemId</span></span> | <span data-ttu-id="ee74b-120">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ee74b-120">String</span></span> | <span data-ttu-id="ee74b-121">Logic app rendszer azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ee74b-121">Logic app system ID.</span></span> <span data-ttu-id="ee74b-122">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-122">(Mandatory)</span></span> |
| <span data-ttu-id="ee74b-123">futtatásazonosító</span><span class="sxs-lookup"><span data-stu-id="ee74b-123">runId</span></span> | <span data-ttu-id="ee74b-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ee74b-124">String</span></span> | <span data-ttu-id="ee74b-125">Logikai alkalmazás futtatásához azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ee74b-125">Logic app run ID.</span></span> <span data-ttu-id="ee74b-126">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-126">(Mandatory)</span></span> |
| <span data-ttu-id="ee74b-127">operationName</span><span class="sxs-lookup"><span data-stu-id="ee74b-127">operationName</span></span> | <span data-ttu-id="ee74b-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ee74b-128">String</span></span> | <span data-ttu-id="ee74b-129">Hello művelet (például a művelet vagy az eseményindító) neve.</span><span class="sxs-lookup"><span data-stu-id="ee74b-129">Name of hello operation (for example, action or trigger).</span></span> <span data-ttu-id="ee74b-130">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-130">(Mandatory)</span></span> |
| <span data-ttu-id="ee74b-131">repeatItemScopeName</span><span class="sxs-lookup"><span data-stu-id="ee74b-131">repeatItemScopeName</span></span> | <span data-ttu-id="ee74b-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ee74b-132">String</span></span> | <span data-ttu-id="ee74b-133">Ismételje meg a elem neve, ha hello művelet belül egy `foreach` / `until` hurok.</span><span class="sxs-lookup"><span data-stu-id="ee74b-133">Repeat item name if hello action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="ee74b-134">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-134">(Mandatory)</span></span> |
| <span data-ttu-id="ee74b-135">repeatItemIndex</span><span class="sxs-lookup"><span data-stu-id="ee74b-135">repeatItemIndex</span></span> | <span data-ttu-id="ee74b-136">Egész szám</span><span class="sxs-lookup"><span data-stu-id="ee74b-136">Integer</span></span> | <span data-ttu-id="ee74b-137">E művelet hello belül van-e egy `foreach` / `until` hurok.</span><span class="sxs-lookup"><span data-stu-id="ee74b-137">Whether hello action is inside a `foreach`/`until` loop.</span></span> <span data-ttu-id="ee74b-138">Azt jelzi, hogy hello ismétlődő elem indexe.</span><span class="sxs-lookup"><span data-stu-id="ee74b-138">Indicates hello repeated item index.</span></span> <span data-ttu-id="ee74b-139">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-139">(Mandatory)</span></span> |
| <span data-ttu-id="ee74b-140">trackingId</span><span class="sxs-lookup"><span data-stu-id="ee74b-140">trackingId</span></span> | <span data-ttu-id="ee74b-141">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ee74b-141">String</span></span> | <span data-ttu-id="ee74b-142">Nyomkövetési azonosító, toocorrelate köszönőüzenetei.</span><span class="sxs-lookup"><span data-stu-id="ee74b-142">Tracking ID, toocorrelate hello messages.</span></span> <span data-ttu-id="ee74b-143">(Választható)</span><span class="sxs-lookup"><span data-stu-id="ee74b-143">(Optional)</span></span> |
| <span data-ttu-id="ee74b-144">correlationId</span><span class="sxs-lookup"><span data-stu-id="ee74b-144">correlationId</span></span> | <span data-ttu-id="ee74b-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ee74b-145">String</span></span> | <span data-ttu-id="ee74b-146">Korrelációs azonosító, toocorrelate köszönőüzenetei.</span><span class="sxs-lookup"><span data-stu-id="ee74b-146">Correlation ID, toocorrelate hello messages.</span></span> <span data-ttu-id="ee74b-147">(Választható)</span><span class="sxs-lookup"><span data-stu-id="ee74b-147">(Optional)</span></span> |
| <span data-ttu-id="ee74b-148">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="ee74b-148">clientRequestId</span></span> | <span data-ttu-id="ee74b-149">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ee74b-149">String</span></span> | <span data-ttu-id="ee74b-150">Ügyfél feltöltheti azt toocorrelate üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="ee74b-150">Client can populate it toocorrelate messages.</span></span> <span data-ttu-id="ee74b-151">(Választható)</span><span class="sxs-lookup"><span data-stu-id="ee74b-151">(Optional)</span></span> |
| <span data-ttu-id="ee74b-152">EventLevel</span><span class="sxs-lookup"><span data-stu-id="ee74b-152">eventLevel</span></span> |   | <span data-ttu-id="ee74b-153">Hello esemény szintje.</span><span class="sxs-lookup"><span data-stu-id="ee74b-153">Level of hello event.</span></span> <span data-ttu-id="ee74b-154">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-154">(Mandatory)</span></span> |
| <span data-ttu-id="ee74b-155">eventTime</span><span class="sxs-lookup"><span data-stu-id="ee74b-155">eventTime</span></span> |   | <span data-ttu-id="ee74b-156">Idő hello esemény, éééé-hh-DDTHH:MM:SS.00000Z UTC formátumban.</span><span class="sxs-lookup"><span data-stu-id="ee74b-156">Time of hello event, in UTC format YYYY-MM-DDTHH:MM:SS.00000Z.</span></span> <span data-ttu-id="ee74b-157">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-157">(Mandatory)</span></span> |
| <span data-ttu-id="ee74b-158">recordType</span><span class="sxs-lookup"><span data-stu-id="ee74b-158">recordType</span></span> |   | <span data-ttu-id="ee74b-159">Hello követése rekord típusa.</span><span class="sxs-lookup"><span data-stu-id="ee74b-159">Type of hello track record.</span></span> <span data-ttu-id="ee74b-160">Az engedélyezett értéket **egyéni**.</span><span class="sxs-lookup"><span data-stu-id="ee74b-160">Allowed value is **custom**.</span></span> <span data-ttu-id="ee74b-161">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-161">(Mandatory)</span></span> |
| <span data-ttu-id="ee74b-162">rekord</span><span class="sxs-lookup"><span data-stu-id="ee74b-162">record</span></span> |   | <span data-ttu-id="ee74b-163">Egyéni rekordtípus.</span><span class="sxs-lookup"><span data-stu-id="ee74b-163">Custom record type.</span></span> <span data-ttu-id="ee74b-164">Engedélyezett JToken érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="ee74b-164">Allowed format is JToken.</span></span> <span data-ttu-id="ee74b-165">(Kötelező)</span><span class="sxs-lookup"><span data-stu-id="ee74b-165">(Mandatory)</span></span> |

## <a name="b2b-protocol-tracking-schemas"></a><span data-ttu-id="ee74b-166">B2B protokoll követési sémák</span><span class="sxs-lookup"><span data-stu-id="ee74b-166">B2B protocol tracking schemas</span></span>
<span data-ttu-id="ee74b-167">Követés sémák B2B protokoll kapcsolatos információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="ee74b-167">For information about B2B protocol tracking schemas, see:</span></span>
* [<span data-ttu-id="ee74b-168">AS2-követési sémák</span><span class="sxs-lookup"><span data-stu-id="ee74b-168">AS2 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [<span data-ttu-id="ee74b-169">X12-követési sémák</span><span class="sxs-lookup"><span data-stu-id="ee74b-169">X12 tracking schemas</span></span>](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a><span data-ttu-id="ee74b-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee74b-170">Next steps</span></span>
* <span data-ttu-id="ee74b-171">További információ [B2B üzenetek figyelése](logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="ee74b-171">Learn more about [monitoring B2B messages](logic-apps-monitor-b2b-message.md).</span></span>   
* <span data-ttu-id="ee74b-172">További tudnivalók [hello Operations Management Suite portálját a B2B üzenetek nyomon követése](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="ee74b-172">Learn about [tracking B2B messages in hello Operations Management Suite portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>
* <span data-ttu-id="ee74b-173">További tudnivalók hello [vállalati integrációs csomag](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ee74b-173">Learn more about hello [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span>
