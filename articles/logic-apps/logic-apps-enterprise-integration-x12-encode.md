---
title: "Kódolja X12 üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "EDI érvényesítése és átalakítani az XML-kódolású X12 helyű üzenet a vállalati integrációs csomag a kódoló Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 29d19364b9a98e351c95f13e68a2e63b9f6439f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="3742f-103">Kódolja X12 üzenetek az Azure Logic Apps a vállalati integrációs csomaggal</span><span class="sxs-lookup"><span data-stu-id="3742f-103">Encode X12 messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="3742f-104">Encode X12 üzenet összekötő EDI és a partner jellemző tulajdonságok ellenőrzése, XML-kódolású üzenetekkel átalakítása EDI tranzakció halmazok cseréje és műszaki nyugtázási vagy működési visszaigazolás kérése.</span><span class="sxs-lookup"><span data-stu-id="3742f-104">With the Encode X12 message connector, you can validate EDI and partner-specific properties, convert XML-encoded messages into EDI transaction sets in the interchange, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="3742f-105">Ez az összekötő használatához fel kell vennie az összekötő egy meglévő elindítani a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3742f-105">To use this connector, you must add the connector to an existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="3742f-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="3742f-106">Before you start</span></span>

<span data-ttu-id="3742f-107">A szükséges elemeket itt található:</span><span class="sxs-lookup"><span data-stu-id="3742f-107">Here's the items you need:</span></span>

* <span data-ttu-id="3742f-108">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="3742f-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="3742f-109">Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="3742f-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="3742f-110">A Encode X12 üzenet összekötő használatához integrációs fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3742f-110">You must have an integration account to use the Encode X12 message connector.</span></span>
* <span data-ttu-id="3742f-111">Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="3742f-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="3742f-112">Egy [X12 megállapodás](logic-apps-enterprise-integration-x12.md) , amely már definiálva van az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="3742f-112">An [X12 agreement](logic-apps-enterprise-integration-x12.md) that's already defined in your integration account</span></span>

## <a name="encode-x12-messages"></a><span data-ttu-id="3742f-113">Kódolja X12 üzenetek</span><span class="sxs-lookup"><span data-stu-id="3742f-113">Encode X12 messages</span></span>

1. <span data-ttu-id="3742f-114">[Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3742f-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="3742f-115">A Encode X12 üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="3742f-115">The Encode X12 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="3742f-116">A Logic App-tervezőben, vegye fel egy eseményindítót, majd egy műveletet a logikai alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="3742f-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="3742f-117">A keresési mezőbe írja be a "x12" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="3742f-117">In the search box, enter "x12" for your filter.</span></span> <span data-ttu-id="3742f-118">Válassza **X12-kódolás X12 szerződésnév üzenetre** vagy **X12-kódolás X12 identitások üzenetből**.</span><span class="sxs-lookup"><span data-stu-id="3742f-118">Select either **X12 - Encode to X12 message by agreement name** or **X12 - Encode to X12 message by identities**.</span></span>
   
    ![Keressen a "x12"](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. <span data-ttu-id="3742f-120">Integráció fiókjába korábban a kapcsolatokat nem hozott létre, ha a program kéri, most, hogy a kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3742f-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="3742f-121">A kapcsolat neve, és válassza ki a integrációs fiókot, amely csatlakozni szeretne.</span><span class="sxs-lookup"><span data-stu-id="3742f-121">Name your connection, and select the integration account that you want to connect.</span></span> 
   
    ![integráció fiók kapcsolat](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    <span data-ttu-id="3742f-123">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="3742f-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="3742f-124">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="3742f-124">Property</span></span> | <span data-ttu-id="3742f-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="3742f-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="3742f-126">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="3742f-126">Connection Name *</span></span> |<span data-ttu-id="3742f-127">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="3742f-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="3742f-128">Integráció fiók *</span><span class="sxs-lookup"><span data-stu-id="3742f-128">Integration Account *</span></span> |<span data-ttu-id="3742f-129">Adja meg a integrációs fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="3742f-129">Enter a name for your integration account.</span></span> <span data-ttu-id="3742f-130">Ellenőrizze, hogy az integrációs fiók és a logikai alkalmazást az Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="3742f-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="3742f-131">Amikor elkészült, a kapcsolódási adatait. Ez a példa hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="3742f-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="3742f-132">Válassza ki a kapcsolat létrehozásának befejezéséhez **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="3742f-132">To finish creating your connection, choose **Create**.</span></span>

    ![integráció fiók kapcsolat létrehozása](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    <span data-ttu-id="3742f-134">A kapcsolat létrejön.</span><span class="sxs-lookup"><span data-stu-id="3742f-134">Your connection is now created.</span></span>

    ![integráció fiók kapcsolódási adatait.](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a><span data-ttu-id="3742f-136">Kódolja X12 szerződés neve üzeneteket</span><span class="sxs-lookup"><span data-stu-id="3742f-136">Encode X12 messages by agreement name</span></span>

<span data-ttu-id="3742f-137">Ha úgy döntött, hogy X12 kódolása üzenetek szerződés neve, nyissa meg a **X12 neve megállapodás** listában, adja meg vagy válassza ki a meglévő X12 szerződést.</span><span class="sxs-lookup"><span data-stu-id="3742f-137">If you chose to encode X12 messages by agreement name, open the **Name of X12 agreement** list, enter or select your existing X12 agreement.</span></span> <span data-ttu-id="3742f-138">Adja meg az XML-üzenet kódolására.</span><span class="sxs-lookup"><span data-stu-id="3742f-138">Enter the XML message to encode.</span></span>

![Adja meg a X12 szerződésnév és kódolni XML-üzenet](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a><span data-ttu-id="3742f-140">Kódolja X12 identitások üzenetek</span><span class="sxs-lookup"><span data-stu-id="3742f-140">Encode X12 messages by identities</span></span>

<span data-ttu-id="3742f-141">Ha úgy dönt, hogy X12 kódolása üzenetek identitások, adja meg a küldő azonosítója, a küldő minősítő, a fogadó azonosítója és a fogadó minősítő a X12 a szerződést.</span><span class="sxs-lookup"><span data-stu-id="3742f-141">If you choose to encode X12 messages by identities, enter the sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your X12 agreement.</span></span> <span data-ttu-id="3742f-142">Válassza ki a kódolni XML üzenetet.</span><span class="sxs-lookup"><span data-stu-id="3742f-142">Select the XML message to encode.</span></span>
   
![Adja meg a küldő és fogadó identitásainak, válassza ki a kódolni XML-üzenet](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a><span data-ttu-id="3742f-144">X12 részletek kódolása</span><span class="sxs-lookup"><span data-stu-id="3742f-144">X12 Encode details</span></span>

<span data-ttu-id="3742f-145">A X12 Encode összekötő az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="3742f-145">The X12 Encode connector performs these tasks:</span></span>

* <span data-ttu-id="3742f-146">A szerződés feloldási összekapcsolja a küldő és fogadó környezeti tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="3742f-146">Agreement resolution by matching sender and receiver context properties.</span></span>
* <span data-ttu-id="3742f-147">XML-kódolású üzenetekkel konvertálásakor EDI tranzakció halmazok adatcsere EDI adatcsere rendezi sorba.</span><span class="sxs-lookup"><span data-stu-id="3742f-147">Serializes the EDI interchange, converting XML-encoded messages into EDI transaction sets in the interchange.</span></span>
* <span data-ttu-id="3742f-148">Tranzakció set fejléc és pótkocsi szegmensek vonatkozik</span><span class="sxs-lookup"><span data-stu-id="3742f-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="3742f-149">Az adatcsere ellenőrző szám, a vezérlő csoportszámmal és a tranzakció beállítása vezérlő száma minden kimenő adatcserét hoz létre</span><span class="sxs-lookup"><span data-stu-id="3742f-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="3742f-150">A felváltja elválasztók a hasznos adatban</span><span class="sxs-lookup"><span data-stu-id="3742f-150">Replaces separators in the payload data</span></span>
* <span data-ttu-id="3742f-151">Érvényesíti EDI- és erőforráspartner jellemző tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="3742f-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="3742f-152">A tranzakció-set adatelemek szemben az üzenet séma séma érvényesítése</span><span class="sxs-lookup"><span data-stu-id="3742f-152">Schema validation of the transaction-set data elements against the message Schema</span></span>
  * <span data-ttu-id="3742f-153">EDI érvényesítési tranzakció-set adatelemek végre.</span><span class="sxs-lookup"><span data-stu-id="3742f-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="3742f-154">Tranzakció-set adatelemek végre kiterjesztett érvényesítése</span><span class="sxs-lookup"><span data-stu-id="3742f-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="3742f-155">A műszaki és/vagy funkcionális visszaigazolás-kérelmek (Ha be van állítva).</span><span class="sxs-lookup"><span data-stu-id="3742f-155">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="3742f-156">A műszaki visszaigazolás miatt érvényesítési fejléc állít elő.</span><span class="sxs-lookup"><span data-stu-id="3742f-156">A Technical Acknowledgment generates as a result of header validation.</span></span> <span data-ttu-id="3742f-157">A műszaki nyugtázást jelentések tartozó az adatcsere fejléc és a cím címzett feldolgozási állapota</span><span class="sxs-lookup"><span data-stu-id="3742f-157">The technical acknowledgment reports the status of the processing of an interchange header and trailer by the address receiver</span></span>
  * <span data-ttu-id="3742f-158">A működési visszaigazolás miatt törzs érvényesítési állít elő.</span><span class="sxs-lookup"><span data-stu-id="3742f-158">A Functional Acknowledgment generates as a result of body validation.</span></span> <span data-ttu-id="3742f-159">A működési nyugtázást jelentést minden egyes hiba történt a kapott dokumentumot</span><span class="sxs-lookup"><span data-stu-id="3742f-159">The functional acknowledgment reports each error encountered while processing the received document</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="3742f-160">A swagger megtekintése</span><span class="sxs-lookup"><span data-stu-id="3742f-160">View the swagger</span></span>
<span data-ttu-id="3742f-161">Tekintse meg a [részletek swagger](/connectors/x12/).</span><span class="sxs-lookup"><span data-stu-id="3742f-161">See the [swagger details](/connectors/x12/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3742f-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3742f-162">Next steps</span></span>
[<span data-ttu-id="3742f-163">További tudnivalók a vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="3742f-163">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

