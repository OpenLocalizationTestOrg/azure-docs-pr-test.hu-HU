---
title: "aaaDecode EDIFACT üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "EDI érvényesítése és nyugtázását a hello EDIFACT üzenet dekóder hello vállalati integrációs csomagot az Azure Logic Apps a"
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
ms.openlocfilehash: 94faebdec4e4ffc8ad76ad1609495ddf9f002250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="1f07f-103">EDIFACT üzenetek dekódolási az Azure Logic Apps a hello vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="1f07f-103">Decode EDIFACT messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="1f07f-104">Hello dekódolása EDIFACT-üzenet összekötővel EDI és a partner jellemző tulajdonságok ellenőrzése, felosztott kereszteződéseket tranzakciók be vagy teljes kereszteződéseket megőrzése és feldolgozott tranzakciók visszaigazoló üzenetet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1f07f-104">With hello Decode EDIFACT message connector, you can validate EDI and partner-specific properties, split interchanges into transactions sets or preserve entire interchanges, and generate acknowledgments for processed transactions.</span></span> <span data-ttu-id="1f07f-105">toouse ezt az összekötőt, hozzá kell adnia a Logic Apps alkalmazást az eseményindító meglévő hello összekötő tooan.</span><span class="sxs-lookup"><span data-stu-id="1f07f-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="1f07f-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="1f07f-106">Before you start</span></span>

<span data-ttu-id="1f07f-107">Itt van szüksége hello elemeket:</span><span class="sxs-lookup"><span data-stu-id="1f07f-107">Here's hello items you need:</span></span>

* <span data-ttu-id="1f07f-108">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="1f07f-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="1f07f-109">Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="1f07f-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="1f07f-110">Az integráció fiók toouse hello dekódolása EDIFACT üzenet csatlakozó kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1f07f-110">You must have an integration account toouse hello Decode EDIFACT message connector.</span></span> 
* <span data-ttu-id="1f07f-111">Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="1f07f-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="1f07f-112">Egy [EDIFACT megállapodás](logic-apps-enterprise-integration-edifact.md) , amely már definiálva van az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="1f07f-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="decode-edifact-messages"></a><span data-ttu-id="1f07f-113">EDIFACT üzenetek dekódolása</span><span class="sxs-lookup"><span data-stu-id="1f07f-113">Decode EDIFACT messages</span></span>

1. <span data-ttu-id="1f07f-114">[Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1f07f-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="1f07f-115">hello dekódolása EDIFACT üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="1f07f-115">hello Decode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="1f07f-116">A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1f07f-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3. <span data-ttu-id="1f07f-117">Hello keresési mezőbe írja be a "EDIFACT" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="1f07f-117">In hello search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="1f07f-118">Válassza ki **EDIFACT üzenet dekódolása**.</span><span class="sxs-lookup"><span data-stu-id="1f07f-118">Select **Decode EDIFACT Message**.</span></span>
   
    ![EDIFACT keresése](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. <span data-ttu-id="1f07f-120">Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="1f07f-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="1f07f-121">A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot.</span><span class="sxs-lookup"><span data-stu-id="1f07f-121">Name your connection, and select hello integration account that you want tooconnect.</span></span>
   
    ![integráció-fiók létrehozása](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    <span data-ttu-id="1f07f-123">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="1f07f-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="1f07f-124">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1f07f-124">Property</span></span> | <span data-ttu-id="1f07f-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="1f07f-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="1f07f-126">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="1f07f-126">Connection Name *</span></span> |<span data-ttu-id="1f07f-127">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="1f07f-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="1f07f-128">Integráció fiók *</span><span class="sxs-lookup"><span data-stu-id="1f07f-128">Integration Account *</span></span> |<span data-ttu-id="1f07f-129">Adja meg a integrációs fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="1f07f-129">Enter a name for your integration account.</span></span> <span data-ttu-id="1f07f-130">Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="1f07f-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

4. <span data-ttu-id="1f07f-131">Amikor a kapcsolat létrehozása toofinish elkészült, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1f07f-131">When you're done toofinish creating your connection, choose **Create**.</span></span> <span data-ttu-id="1f07f-132">A kapcsolat adatai alábbihoz hasonló toothis példa:</span><span class="sxs-lookup"><span data-stu-id="1f07f-132">Your connection details should look similar toothis example:</span></span>

    ![integráció fiókadatok](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. <span data-ttu-id="1f07f-134">Miután létrehozta a kapcsolatot, ebben a példában látható módon, válassza ki a hello EDIFACT egybesimított fájl üzenet toodecode.</span><span class="sxs-lookup"><span data-stu-id="1f07f-134">After your connection is created, as shown in this example, select hello EDIFACT flat file message toodecode.</span></span>

    ![integráció fiók kapcsolat létrehozása](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    <span data-ttu-id="1f07f-136">Példa:</span><span class="sxs-lookup"><span data-stu-id="1f07f-136">For example:</span></span>

    ![EDIFACT egybesimított fájl üzenet dekódolását kiválasztása](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a><span data-ttu-id="1f07f-138">EDIFACT dekóder részletei</span><span class="sxs-lookup"><span data-stu-id="1f07f-138">EDIFACT decoder details</span></span>

<span data-ttu-id="1f07f-139">hello dekódolása EDIFACT-összekötő az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="1f07f-139">hello Decode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="1f07f-140">Hello boríték kereskedelmipartner-egyezmény szemben érvényesíti.</span><span class="sxs-lookup"><span data-stu-id="1f07f-140">Validates hello envelope against trading partner agreement.</span></span>
* <span data-ttu-id="1f07f-141">Oldja fel a hello megállapodás egyeztetésével hello küldő minősítő & azonosítója és a fogadó minősítő & azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1f07f-141">Resolves hello agreement by matching hello sender qualifier & identifier and receiver qualifier & identifier.</span></span>
* <span data-ttu-id="1f07f-142">Ha az hello interchange fogadni a konfigurációs beállítások több tranzakció hello megállapodás alapján felosztja a több tranzakcióra cseréje.</span><span class="sxs-lookup"><span data-stu-id="1f07f-142">Splits an interchange into multiple transactions when hello interchange has more than one transaction based on hello agreement's receive settings configuration.</span></span>
* <span data-ttu-id="1f07f-143">Hello interchange visszafejti.</span><span class="sxs-lookup"><span data-stu-id="1f07f-143">Disassembles hello interchange.</span></span>
* <span data-ttu-id="1f07f-144">Ellenőrzi az EDI és a partner jellemző tulajdonságokat, beleértve:</span><span class="sxs-lookup"><span data-stu-id="1f07f-144">Validates EDI and partner-specific properties including:</span></span>
  * <span data-ttu-id="1f07f-145">Hello interchange boríték struktúra érvényesítése</span><span class="sxs-lookup"><span data-stu-id="1f07f-145">Validation of hello interchange envelope structure</span></span>
  * <span data-ttu-id="1f07f-146">A sémaérvényesítés hello boríték hello vezérlő séma</span><span class="sxs-lookup"><span data-stu-id="1f07f-146">Schema validation of hello envelope against hello control schema</span></span>
  * <span data-ttu-id="1f07f-147">A sémaérvényesítés hello tranzakció-set adatelemek hello üzenet séma</span><span class="sxs-lookup"><span data-stu-id="1f07f-147">Schema validation of hello transaction-set data elements against hello message schema</span></span>
  * <span data-ttu-id="1f07f-148">Tranzakció-set adatelemek végre EDI érvényesítése</span><span class="sxs-lookup"><span data-stu-id="1f07f-148">EDI validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="1f07f-149">Ellenőrzi, hogy hello interchange, a csoport és a tranzakció set vezérlő számok nem ismétlődő (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="1f07f-149">Verifies that hello interchange, group, and transaction set control numbers are not duplicates (if configured)</span></span> 
  * <span data-ttu-id="1f07f-150">Ellenőrzi, hello interchange ellenőrző szám korábban fogadott kereszteződéseket ellen.</span><span class="sxs-lookup"><span data-stu-id="1f07f-150">Checks hello interchange control number against previously received interchanges.</span></span> 
  * <span data-ttu-id="1f07f-151">Hello csoport ellenőrző szám más hello interchange csoport vezérlő számoknak ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="1f07f-151">Checks hello group control number against other group control numbers in hello interchange.</span></span> 
  * <span data-ttu-id="1f07f-152">Ellenőrzi, hogy hello tranzakció beállítása vezérlő számoknak más tranzakció set vezérlő csoport.</span><span class="sxs-lookup"><span data-stu-id="1f07f-152">Checks hello transaction set control number against other transaction set control numbers in that group.</span></span>
* <span data-ttu-id="1f07f-153">Felosztja a hello interchange tranzakció be, vagy megőrzi a teljes adatcsere hello:</span><span class="sxs-lookup"><span data-stu-id="1f07f-153">Splits hello interchange into transaction sets, or preserves hello entire interchange:</span></span>
  * <span data-ttu-id="1f07f-154">Vegyes Interchange, tranzakció készletek - tranzakció készletek felfüggeszteni hiba: elágazást interchange tranzakcióban állítja be, és minden tranzakció set elemzi.</span><span class="sxs-lookup"><span data-stu-id="1f07f-154">Split Interchange as transaction sets - suspend transaction sets on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="1f07f-155">hello X12 dekódolási művelet kimenetében csak tranzakció ezekről érvényesítése túl teljesítő`badMessages`, és kiírja a fennmaradó tranzakciók hello beállítása túl`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="1f07f-155">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="1f07f-156">Vegyes Interchange, tranzakció készletek - interchange felfüggeszteni hiba: elágazást interchange tranzakcióban állítja be, és minden tranzakció set elemzi.</span><span class="sxs-lookup"><span data-stu-id="1f07f-156">Split Interchange as transaction sets - suspend interchange on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="1f07f-157">Ha egy vagy több tranzakció hello interchange sikertelen érvényesítési állítja, hello X12 dekódolási művelet kimenetében összes hello tranzakció túl beállítja, hogy interchange`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="1f07f-157">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span>
  * <span data-ttu-id="1f07f-158">Megőrizheti a adatcsere - tranzakció készletek felfüggeszteni hiba: Preserve hello adatcsere és a folyamat hello teljes kötegelt adatcserét.</span><span class="sxs-lookup"><span data-stu-id="1f07f-158">Preserve Interchange - suspend transaction sets on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="1f07f-159">hello X12 dekódolási művelet kimenetében csak tranzakció ezekről érvényesítése túl teljesítő`badMessages`, és kiírja a fennmaradó tranzakciók hello beállítása túl`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="1f07f-159">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="1f07f-160">Megőrizheti a adatcsere - interchange felfüggeszteni hiba: Preserve hello adatcsere és a folyamat hello teljes kötegelt adatcserét.</span><span class="sxs-lookup"><span data-stu-id="1f07f-160">Preserve Interchange - suspend interchange on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="1f07f-161">Ha egy vagy több tranzakció hello interchange sikertelen érvényesítési állítja, hello X12 dekódolási művelet kimenetében összes hello tranzakció túl beállítja, hogy interchange`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="1f07f-161">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span>
* <span data-ttu-id="1f07f-162">(Ha be van állítva) hoz létre egy műszaki (vezérlő) és/vagy a működési visszaigazolás.</span><span class="sxs-lookup"><span data-stu-id="1f07f-162">Generates a Technical (control) and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="1f07f-163">Egy technikai nyugtázások vagy hello CONTRL ACK jelentések hello teljes kapott interchange szintaktikai ellenőrzése hello eredményeit.</span><span class="sxs-lookup"><span data-stu-id="1f07f-163">A Technical Acknowledgment or hello CONTRL ACK reports hello results of a syntactical check of hello complete received interchange.</span></span>
  * <span data-ttu-id="1f07f-164">A működési visszaigazolás elismeri elfogadására vagy elutasítására fogadott interchange vagy felhasználói csoport</span><span class="sxs-lookup"><span data-stu-id="1f07f-164">A functional acknowledgment acknowledges accept or reject a received interchange or a group</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="1f07f-165">A Swagger-fájl megtekintése</span><span class="sxs-lookup"><span data-stu-id="1f07f-165">View Swagger file</span></span>
<span data-ttu-id="1f07f-166">tooview hello Swagger részletek hello EDIFACT-összekötőhöz: [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="1f07f-166">tooview hello Swagger details for hello EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f07f-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1f07f-167">Next steps</span></span>
[<span data-ttu-id="1f07f-168">További tudnivalók a vállalati integrációs csomag hello</span><span class="sxs-lookup"><span data-stu-id="1f07f-168">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

