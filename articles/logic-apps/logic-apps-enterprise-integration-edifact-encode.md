---
title: "EDIFACT üzenetek - Azure Logic Apps kódolása |} Microsoft Docs"
description: "EDI érvényesítése és EDIFACT üzenetkódoló a vállalati integrációs csomagban az XML létrehozása az Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: b8d577326d23ec45cb4a9ec0e450ebf7afd945f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="911d6-103">Az Azure Logic Apps a vállalati integrációs csomaggal EDIFACT üzenetek kódolása</span><span class="sxs-lookup"><span data-stu-id="911d6-103">Encode EDIFACT messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="911d6-104">EDIFACT kódolása üzenet összekötő EDI és a partner jellemző tulajdonságok ellenőrzése, az XML-dokumentum az egyes tranzakciót létrehozni és műszaki nyugtázási vagy működési visszaigazolás kérése.</span><span class="sxs-lookup"><span data-stu-id="911d6-104">With the Encode EDIFACT message connector, you can validate EDI and partner-specific properties, generate an XML document for each transaction set, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="911d6-105">Ez az összekötő használatához fel kell vennie az összekötő egy meglévő elindítani a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="911d6-105">To use this connector, you must add the connector to an existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="911d6-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="911d6-106">Before you start</span></span>

<span data-ttu-id="911d6-107">A szükséges elemeket itt található:</span><span class="sxs-lookup"><span data-stu-id="911d6-107">Here's the items you need:</span></span>

* <span data-ttu-id="911d6-108">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="911d6-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="911d6-109">Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="911d6-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="911d6-110">A kódolás EDIFACT üzenet összekötő használatához integrációs fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="911d6-110">You must have an integration account to use the Encode EDIFACT message connector.</span></span> 
* <span data-ttu-id="911d6-111">Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="911d6-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="911d6-112">Egy [EDIFACT megállapodás](logic-apps-enterprise-integration-edifact.md) , amely már definiálva van az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="911d6-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="encode-edifact-messages"></a><span data-ttu-id="911d6-113">EDIFACT üzenetek kódolása</span><span class="sxs-lookup"><span data-stu-id="911d6-113">Encode EDIFACT messages</span></span>

1. <span data-ttu-id="911d6-114">[Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="911d6-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="911d6-115">A kódolás EDIFACT üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="911d6-115">The Encode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="911d6-116">A Logic App-tervezőben, vegye fel egy eseményindítót, majd egy műveletet a logikai alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="911d6-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="911d6-117">A keresési mezőbe írja be a "EDIFACT" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="911d6-117">In the search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="911d6-118">Válassza **EDIFACT üzenetből kódolása szerződésnév** vagy **Encode EDIFACT üzenet által identitások**.</span><span class="sxs-lookup"><span data-stu-id="911d6-118">Select either **Encode EDIFACT Message by agreement name** or **Encode to EDIFACT message by identities**.</span></span>
   
    ![EDIFACT keresése](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. <span data-ttu-id="911d6-120">Integráció fiókjába korábban a kapcsolatokat nem hozott létre, ha a program kéri, most, hogy a kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="911d6-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="911d6-121">A kapcsolat neve, és válassza ki a integrációs fiókot, amely csatlakozni szeretne.</span><span class="sxs-lookup"><span data-stu-id="911d6-121">Name your connection, and select the integration account that you want to connect.</span></span>

    ![integráció fiók kapcsolat létrehozása](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    <span data-ttu-id="911d6-123">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="911d6-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="911d6-124">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="911d6-124">Property</span></span> | <span data-ttu-id="911d6-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="911d6-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="911d6-126">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="911d6-126">Connection Name *</span></span> |<span data-ttu-id="911d6-127">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="911d6-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="911d6-128">Integráció fiók *</span><span class="sxs-lookup"><span data-stu-id="911d6-128">Integration Account *</span></span> |<span data-ttu-id="911d6-129">Adja meg a integrációs fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="911d6-129">Enter a name for your integration account.</span></span> <span data-ttu-id="911d6-130">Ellenőrizze, hogy az integrációs fiók és a logikai alkalmazást az Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="911d6-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="911d6-131">Amikor elkészült, a kapcsolódási adatait. Ez a példa hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="911d6-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="911d6-132">Válassza ki a kapcsolat létrehozásának befejezéséhez **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="911d6-132">To finish creating your connection, choose **Create**.</span></span>

    ![integráció fiók kapcsolódási adatait.](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    <span data-ttu-id="911d6-134">A kapcsolat létrejön.</span><span class="sxs-lookup"><span data-stu-id="911d6-134">Your connection is now created.</span></span>

    ![integráció fiók kapcsolat létrehozása](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a><span data-ttu-id="911d6-136">EDIFACT üzenetek kódolása szerződés neve</span><span class="sxs-lookup"><span data-stu-id="911d6-136">Encode EDIFACT Message by agreement name</span></span>

<span data-ttu-id="911d6-137">Ha úgy döntött, hogy EDIFACT üzenetek kódolása szerződés neve, nyissa meg a **nevét a EDIFACT megállapodás** listában, adja meg vagy válassza ki a EDIFACT a szerződés nevét.</span><span class="sxs-lookup"><span data-stu-id="911d6-137">If you chose to encode EDIFACT messages by agreement name, open the **Name of EDIFACT agreement** list, enter or select your EDIFACT agreement name.</span></span> <span data-ttu-id="911d6-138">Adja meg az XML-üzenet kódolására.</span><span class="sxs-lookup"><span data-stu-id="911d6-138">Enter the XML message to encode.</span></span>

![Adja meg a EDIFACT szerződés neve és kódolni XML-üzenet](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a><span data-ttu-id="911d6-140">EDIFACT üzenetek által identitások kódolása</span><span class="sxs-lookup"><span data-stu-id="911d6-140">Encode EDIFACT Message by identities</span></span>

<span data-ttu-id="911d6-141">Ha EDIFACT üzenetek kódolása által identitások választja, adja meg a küldő azonosítója, a küldő minősítő, a fogadó azonosítója és a fogadó minősítő a EDIFACT megállapodás be.</span><span class="sxs-lookup"><span data-stu-id="911d6-141">If you choose to encode EDIFACT messages by identities, enter the sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your EDIFACT agreement.</span></span> <span data-ttu-id="911d6-142">Válassza ki a kódolni XML üzenetet.</span><span class="sxs-lookup"><span data-stu-id="911d6-142">Select the XML message to encode.</span></span>

![Adja meg a küldő és fogadó identitásainak, válassza ki a kódolni XML-üzenet](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a><span data-ttu-id="911d6-144">EDIFACT kódolása részletei</span><span class="sxs-lookup"><span data-stu-id="911d6-144">EDIFACT Encode details</span></span>

<span data-ttu-id="911d6-145">A kódolási EDIFACT-összekötő az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="911d6-145">The Encode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="911d6-146">A szerződés feloldani a küldő minősítő & azonosítója és a fogadó minősítő és az azonosítója</span><span class="sxs-lookup"><span data-stu-id="911d6-146">Resolve the agreement by matching the sender qualifier & identifier and receiver qualifier and identifier</span></span>
* <span data-ttu-id="911d6-147">XML-kódolású üzenetekkel konvertálásakor EDI tranzakció halmazok adatcsere EDI adatcsere rendezi sorba.</span><span class="sxs-lookup"><span data-stu-id="911d6-147">Serializes the EDI interchange, converting XML-encoded messages into EDI transaction sets in the interchange.</span></span>
* <span data-ttu-id="911d6-148">Tranzakció set fejléc és pótkocsi szegmensek vonatkozik</span><span class="sxs-lookup"><span data-stu-id="911d6-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="911d6-149">Az adatcsere ellenőrző szám, a vezérlő csoportszámmal és a tranzakció beállítása vezérlő száma minden kimenő adatcserét hoz létre</span><span class="sxs-lookup"><span data-stu-id="911d6-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="911d6-150">A felváltja elválasztók a hasznos adatban</span><span class="sxs-lookup"><span data-stu-id="911d6-150">Replaces separators in the payload data</span></span>
* <span data-ttu-id="911d6-151">Érvényesíti EDI- és erőforráspartner jellemző tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="911d6-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="911d6-152">A sémaérvényesítés a tranzakció-set adatelemek az üzenet sémának.</span><span class="sxs-lookup"><span data-stu-id="911d6-152">Schema validation of the transaction-set data elements against the message schema.</span></span>
  * <span data-ttu-id="911d6-153">EDI érvényesítési tranzakció-set adatelemek végre.</span><span class="sxs-lookup"><span data-stu-id="911d6-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="911d6-154">Tranzakció-set adatelemek végre kiterjesztett érvényesítése</span><span class="sxs-lookup"><span data-stu-id="911d6-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="911d6-155">Az XML-dokumentum az egyes tranzakció állít elő.</span><span class="sxs-lookup"><span data-stu-id="911d6-155">Generates an XML document for each transaction set.</span></span>
* <span data-ttu-id="911d6-156">A műszaki és/vagy funkcionális visszaigazolás-kérelmek (Ha be van állítva).</span><span class="sxs-lookup"><span data-stu-id="911d6-156">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="911d6-157">A műszaki visszaigazolás, mint a CONTRL üzenet azt jelzi, az cseréje fogadását.</span><span class="sxs-lookup"><span data-stu-id="911d6-157">As a technical acknowledgment, the CONTRL message indicates receipt of an interchange.</span></span>
  * <span data-ttu-id="911d6-158">A működési visszaigazolás, mint a CONTRL az üzenet azt jelzi, elfogadása vagy elutasítása a fogadott interchange, csoport vagy üzenet, hibák vagy nem támogatott funkciók listáját</span><span class="sxs-lookup"><span data-stu-id="911d6-158">As a functional acknowledgment, the CONTRL message indicates acceptance or rejection of the received interchange, group, or message, with a list of errors or unsupported functionality</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="911d6-159">A Swagger-fájl megtekintése</span><span class="sxs-lookup"><span data-stu-id="911d6-159">View Swagger file</span></span>
<span data-ttu-id="911d6-160">A Swagger adatai a EDIFACT-összekötő megtekintése: [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="911d6-160">To view the Swagger details for the EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="911d6-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="911d6-161">Next steps</span></span>
[<span data-ttu-id="911d6-162">További tudnivalók a vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="911d6-162">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

