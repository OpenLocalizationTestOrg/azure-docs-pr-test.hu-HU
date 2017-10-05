---
title: "EDIFACT üzenetek - Azure Logic Apps dekódolása |} Microsoft Docs"
description: "EDI érvényesítése, és a vállalati integrációs csomag a EDIFACT üzenet dekóder a nyugtázásokhoz létrehozni az Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: e3787b48037360bf6066ddce2bacba6842213b2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="27eb7-103">Az Azure Logic Apps a vállalati integrációs csomaggal EDIFACT üzenetek dekódolása</span><span class="sxs-lookup"><span data-stu-id="27eb7-103">Decode EDIFACT messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="27eb7-104">Dekódolás EDIFACT üzenet összekötő EDI és a partner jellemző tulajdonságok ellenőrzése, felosztott kereszteződéseket tranzakciók be vagy teljes kereszteződéseket megőrzése és feldolgozott tranzakciók visszaigazoló üzenetet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="27eb7-104">With the Decode EDIFACT message connector, you can validate EDI and partner-specific properties, split interchanges into transactions sets or preserve entire interchanges, and generate acknowledgments for processed transactions.</span></span> <span data-ttu-id="27eb7-105">Ez az összekötő használatához fel kell vennie az összekötő egy meglévő elindítani a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="27eb7-105">To use this connector, you must add the connector to an existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="27eb7-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="27eb7-106">Before you start</span></span>

<span data-ttu-id="27eb7-107">A szükséges elemeket itt található:</span><span class="sxs-lookup"><span data-stu-id="27eb7-107">Here's the items you need:</span></span>

* <span data-ttu-id="27eb7-108">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="27eb7-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="27eb7-109">Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="27eb7-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="27eb7-110">A dekódolási EDIFACT üzenet összekötő használatához integrációs fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="27eb7-110">You must have an integration account to use the Decode EDIFACT message connector.</span></span> 
* <span data-ttu-id="27eb7-111">Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="27eb7-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="27eb7-112">Egy [EDIFACT megállapodás](logic-apps-enterprise-integration-edifact.md) , amely már definiálva van az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="27eb7-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="decode-edifact-messages"></a><span data-ttu-id="27eb7-113">EDIFACT üzenetek dekódolása</span><span class="sxs-lookup"><span data-stu-id="27eb7-113">Decode EDIFACT messages</span></span>

1. <span data-ttu-id="27eb7-114">[Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="27eb7-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="27eb7-115">A dekódolási EDIFACT üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="27eb7-115">The Decode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="27eb7-116">A Logic App-tervezőben, vegye fel egy eseményindítót, majd egy műveletet a logikai alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="27eb7-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3. <span data-ttu-id="27eb7-117">A keresési mezőbe írja be a "EDIFACT" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="27eb7-117">In the search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="27eb7-118">Válassza ki **EDIFACT üzenet dekódolása**.</span><span class="sxs-lookup"><span data-stu-id="27eb7-118">Select **Decode EDIFACT Message**.</span></span>
   
    ![EDIFACT keresése](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. <span data-ttu-id="27eb7-120">Integráció fiókjába korábban a kapcsolatokat nem hozott létre, ha a program kéri, most, hogy a kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="27eb7-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="27eb7-121">A kapcsolat neve, és válassza ki a integrációs fiókot, amely csatlakozni szeretne.</span><span class="sxs-lookup"><span data-stu-id="27eb7-121">Name your connection, and select the integration account that you want to connect.</span></span>
   
    ![integráció-fiók létrehozása](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    <span data-ttu-id="27eb7-123">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="27eb7-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="27eb7-124">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="27eb7-124">Property</span></span> | <span data-ttu-id="27eb7-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="27eb7-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="27eb7-126">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="27eb7-126">Connection Name *</span></span> |<span data-ttu-id="27eb7-127">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="27eb7-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="27eb7-128">Integráció fiók *</span><span class="sxs-lookup"><span data-stu-id="27eb7-128">Integration Account *</span></span> |<span data-ttu-id="27eb7-129">Adja meg a integrációs fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="27eb7-129">Enter a name for your integration account.</span></span> <span data-ttu-id="27eb7-130">Ellenőrizze, hogy az integrációs fiók és a logikai alkalmazást az Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="27eb7-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

4. <span data-ttu-id="27eb7-131">Amikor elkészült, a hozta létre a kapcsolat válasszon **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="27eb7-131">When you're done to finish creating your connection, choose **Create**.</span></span> <span data-ttu-id="27eb7-132">A kapcsolódási adatait. Ez a példa hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="27eb7-132">Your connection details should look similar to this example:</span></span>

    ![integráció fiókadatok](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. <span data-ttu-id="27eb7-134">Miután létrehozta a kapcsolatot, ebben a példában látható módon, válassza ki a EDIFACT egybesimított fájl üzenetet dekódolására.</span><span class="sxs-lookup"><span data-stu-id="27eb7-134">After your connection is created, as shown in this example, select the EDIFACT flat file message to decode.</span></span>

    ![integráció fiók kapcsolat létrehozása](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    <span data-ttu-id="27eb7-136">Példa:</span><span class="sxs-lookup"><span data-stu-id="27eb7-136">For example:</span></span>

    ![EDIFACT egybesimított fájl üzenet dekódolását kiválasztása](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a><span data-ttu-id="27eb7-138">EDIFACT dekóder részletei</span><span class="sxs-lookup"><span data-stu-id="27eb7-138">EDIFACT decoder details</span></span>

<span data-ttu-id="27eb7-139">Az dekódolása EDIFACT-összekötő az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="27eb7-139">The Decode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="27eb7-140">A boríték kereskedelmipartner-egyezmény szemben érvényesíti.</span><span class="sxs-lookup"><span data-stu-id="27eb7-140">Validates the envelope against trading partner agreement.</span></span>
* <span data-ttu-id="27eb7-141">A szerződés megszünteti a küldő minősítő & azonosítója és a fogadó minősítő & azonosítója egyeztetve.</span><span class="sxs-lookup"><span data-stu-id="27eb7-141">Resolves the agreement by matching the sender qualifier & identifier and receiver qualifier & identifier.</span></span>
* <span data-ttu-id="27eb7-142">Ha az adatcsere fogadni a konfigurációs beállítások több tranzakció a szerződés alapján felosztja a több tranzakcióra cseréje.</span><span class="sxs-lookup"><span data-stu-id="27eb7-142">Splits an interchange into multiple transactions when the interchange has more than one transaction based on the agreement's receive settings configuration.</span></span>
* <span data-ttu-id="27eb7-143">Visszafejti a adatcserét.</span><span class="sxs-lookup"><span data-stu-id="27eb7-143">Disassembles the interchange.</span></span>
* <span data-ttu-id="27eb7-144">Ellenőrzi az EDI és a partner jellemző tulajdonságokat, beleértve:</span><span class="sxs-lookup"><span data-stu-id="27eb7-144">Validates EDI and partner-specific properties including:</span></span>
  * <span data-ttu-id="27eb7-145">Az adatcsere boríték struktúra érvényesítése</span><span class="sxs-lookup"><span data-stu-id="27eb7-145">Validation of the interchange envelope structure</span></span>
  * <span data-ttu-id="27eb7-146">A vezérlő sémának a boríték séma érvényesítése</span><span class="sxs-lookup"><span data-stu-id="27eb7-146">Schema validation of the envelope against the control schema</span></span>
  * <span data-ttu-id="27eb7-147">A tranzakció-set adatelemek az üzenet sémán elvégzett séma érvényesítése</span><span class="sxs-lookup"><span data-stu-id="27eb7-147">Schema validation of the transaction-set data elements against the message schema</span></span>
  * <span data-ttu-id="27eb7-148">Tranzakció-set adatelemek végre EDI érvényesítése</span><span class="sxs-lookup"><span data-stu-id="27eb7-148">EDI validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="27eb7-149">Ellenőrzi, hogy a csomópont, a csoport és a tranzakció set vezérlő számok nem ismétlődő (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="27eb7-149">Verifies that the interchange, group, and transaction set control numbers are not duplicates (if configured)</span></span> 
  * <span data-ttu-id="27eb7-150">Ellenőrzi a interchange vezérlő korábban fogadott kereszteződéseket ellen.</span><span class="sxs-lookup"><span data-stu-id="27eb7-150">Checks the interchange control number against previously received interchanges.</span></span> 
  * <span data-ttu-id="27eb7-151">A vezérlő csoportszám más cseréje a csoport vezérlő számoknak ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="27eb7-151">Checks the group control number against other group control numbers in the interchange.</span></span> 
  * <span data-ttu-id="27eb7-152">Ellenőrzi, hogy a tranzakció beállítása vezérlő számoknak más tranzakció set vezérlő csoport.</span><span class="sxs-lookup"><span data-stu-id="27eb7-152">Checks the transaction set control number against other transaction set control numbers in that group.</span></span>
* <span data-ttu-id="27eb7-153">Felosztja a cseréje a tranzakció be, vagy a teljes adatcsere megőrzi:</span><span class="sxs-lookup"><span data-stu-id="27eb7-153">Splits the interchange into transaction sets, or preserves the entire interchange:</span></span>
  * <span data-ttu-id="27eb7-154">Vegyes Interchange, tranzakció készletek - tranzakció készletek felfüggeszteni hiba: elágazást interchange tranzakcióban állítja be, és minden tranzakció set elemzi.</span><span class="sxs-lookup"><span data-stu-id="27eb7-154">Split Interchange as transaction sets - suspend transaction sets on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="27eb7-155">A X12 dekódolási művelet kimenetében csak azokat a tranzakciót, amely beállítja a érvényesítése sikertelen `badMessages`, és beállítja a fennmaradó tranzakciók kimenetek `goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="27eb7-155">The X12 Decode action outputs only those transaction sets that fail validation to `badMessages`, and outputs the remaining transactions sets to `goodMessages`.</span></span>
  * <span data-ttu-id="27eb7-156">Vegyes Interchange, tranzakció készletek - interchange felfüggeszteni hiba: elágazást interchange tranzakcióban állítja be, és minden tranzakció set elemzi.</span><span class="sxs-lookup"><span data-stu-id="27eb7-156">Split Interchange as transaction sets - suspend interchange on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="27eb7-157">Ha egy vagy több tranzakció állítja, az adatcsere ellenőrzésen, a X12 dekódolási művelet kimenetében, a tranzakció beállítja, hogy a csomópont nem `badMessages`.</span><span class="sxs-lookup"><span data-stu-id="27eb7-157">If one or more transaction sets in the interchange fail validation, the X12 Decode action outputs all the transaction sets in that interchange to `badMessages`.</span></span>
  * <span data-ttu-id="27eb7-158">Megőrizheti a adatcsere - tranzakció készletek felfüggeszteni hiba: az adatcsere megőrzése, és a teljes kötegelt interchange feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="27eb7-158">Preserve Interchange - suspend transaction sets on error: Preserve the interchange and process the entire batched interchange.</span></span> 
  <span data-ttu-id="27eb7-159">A X12 dekódolási művelet kimenetében csak azokat a tranzakciót, amely beállítja a érvényesítése sikertelen `badMessages`, és beállítja a fennmaradó tranzakciók kimenetek `goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="27eb7-159">The X12 Decode action outputs only those transaction sets that fail validation to `badMessages`, and outputs the remaining transactions sets to `goodMessages`.</span></span>
  * <span data-ttu-id="27eb7-160">Megőrizheti a adatcsere - interchange felfüggeszteni hiba: az adatcsere megőrzése, és a teljes kötegelt interchange feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="27eb7-160">Preserve Interchange - suspend interchange on error: Preserve the interchange and process the entire batched interchange.</span></span> 
  <span data-ttu-id="27eb7-161">Ha egy vagy több tranzakció állítja, az adatcsere ellenőrzésen, a X12 dekódolási művelet kimenetében, a tranzakció beállítja, hogy a csomópont nem `badMessages`.</span><span class="sxs-lookup"><span data-stu-id="27eb7-161">If one or more transaction sets in the interchange fail validation, the X12 Decode action outputs all the transaction sets in that interchange to `badMessages`.</span></span>
* <span data-ttu-id="27eb7-162">(Ha be van állítva) hoz létre egy műszaki (vezérlő) és/vagy a működési visszaigazolás.</span><span class="sxs-lookup"><span data-stu-id="27eb7-162">Generates a Technical (control) and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="27eb7-163">A műszaki visszaigazolás vagy a CONTRL nyugtát KAPNI a jelentés teljes fogadott adatcsere szintaktikai ellenőrzés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="27eb7-163">A Technical Acknowledgment or the CONTRL ACK reports the results of a syntactical check of the complete received interchange.</span></span>
  * <span data-ttu-id="27eb7-164">A működési visszaigazolás elismeri elfogadására vagy elutasítására fogadott interchange vagy felhasználói csoport</span><span class="sxs-lookup"><span data-stu-id="27eb7-164">A functional acknowledgment acknowledges accept or reject a received interchange or a group</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="27eb7-165">A Swagger-fájl megtekintése</span><span class="sxs-lookup"><span data-stu-id="27eb7-165">View Swagger file</span></span>
<span data-ttu-id="27eb7-166">A Swagger adatai a EDIFACT-összekötő megtekintése: [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="27eb7-166">To view the Swagger details for the EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="27eb7-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="27eb7-167">Next steps</span></span>
[<span data-ttu-id="27eb7-168">További tudnivalók a vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="27eb7-168">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

