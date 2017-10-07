---
title: "aaaDecode X12 üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "EDI érvényesítése és nyugtázását a hello X12 üzenet dekóder hello vállalati integrációs csomagot az Azure Logic Apps a"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 1ffececca1ff835b319b64c85f86c421395833c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="decode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="09e61-103">Dekódolás X12 hello vállalati integrációs csomagot az Azure Logic Apps állapotüzenete</span><span class="sxs-lookup"><span data-stu-id="09e61-103">Decode X12 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="09e61-104">Hello dekódolási X12 üzenet összekötővel hello boríték elleni kereskedelmipartner-egyezmény érvényesítése, EDI és a partner jellemző tulajdonságok ellenőrzése, felosztott kereszteződéseket tranzakciók be vagy teljes kereszteződéseket megőrzése, és beállíthatja készítése feldolgozott tranzakciók visszaigazoló üzenetet.</span><span class="sxs-lookup"><span data-stu-id="09e61-104">With hello Decode X12 message connector, you can validate hello envelope against a trading partner agreement, validate EDI and partner-specific properties, split interchanges into transactions sets or preserve entire interchanges, and generate acknowledgments for processed transactions.</span></span> <span data-ttu-id="09e61-105">toouse ezt az összekötőt, hozzá kell adnia a Logic Apps alkalmazást az eseményindító meglévő hello összekötő tooan.</span><span class="sxs-lookup"><span data-stu-id="09e61-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="09e61-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="09e61-106">Before you start</span></span>

<span data-ttu-id="09e61-107">Itt van szüksége hello elemeket:</span><span class="sxs-lookup"><span data-stu-id="09e61-107">Here's hello items you need:</span></span>

* <span data-ttu-id="09e61-108">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="09e61-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="09e61-109">Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="09e61-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="09e61-110">Az integráció fiók toouse hello dekódolási X12 üzenet csatlakozó kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="09e61-110">You must have an integration account toouse hello Decode X12 message connector.</span></span>
* <span data-ttu-id="09e61-111">Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="09e61-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="09e61-112">Egy [X12 megállapodás](logic-apps-enterprise-integration-x12.md) , amely már definiálva van az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="09e61-112">An [X12 agreement](logic-apps-enterprise-integration-x12.md) that's already defined in your integration account</span></span>

## <a name="decode-x12-messages"></a><span data-ttu-id="09e61-113">Dekódolás X12 üzenetek</span><span class="sxs-lookup"><span data-stu-id="09e61-113">Decode X12 messages</span></span>

1. <span data-ttu-id="09e61-114">[Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="09e61-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="09e61-115">hello dekódolási X12 üzenet összekötő eseményindítókat, nem rendelkezik, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például egy kérelem eseményindító.</span><span class="sxs-lookup"><span data-stu-id="09e61-115">hello Decode X12 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="09e61-116">A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="09e61-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="09e61-117">Hello keresési mezőbe írja be a "x12" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="09e61-117">In hello search box, enter "x12" for your filter.</span></span> <span data-ttu-id="09e61-118">Válassza ki **X12-dekódolási X12 üzenet**.</span><span class="sxs-lookup"><span data-stu-id="09e61-118">Select **X12 - Decode X12 message**.</span></span>
   
    ![Keressen a "x12"](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. <span data-ttu-id="09e61-120">Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="09e61-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="09e61-121">A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot.</span><span class="sxs-lookup"><span data-stu-id="09e61-121">Name your connection, and select hello integration account that you want tooconnect.</span></span> 

    ![Adja meg az integrációs fiók kapcsolódási adatait.](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    <span data-ttu-id="09e61-123">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="09e61-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="09e61-124">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="09e61-124">Property</span></span> | <span data-ttu-id="09e61-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="09e61-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="09e61-126">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="09e61-126">Connection Name *</span></span> |<span data-ttu-id="09e61-127">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="09e61-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="09e61-128">Integráció fiók *</span><span class="sxs-lookup"><span data-stu-id="09e61-128">Integration Account *</span></span> |<span data-ttu-id="09e61-129">Adja meg a integrációs fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="09e61-129">Enter a name for your integration account.</span></span> <span data-ttu-id="09e61-130">Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="09e61-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="09e61-131">Amikor elkészült, a kapcsolat adatai alábbihoz hasonló toothis példa.</span><span class="sxs-lookup"><span data-stu-id="09e61-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="09e61-132">a kapcsolat létrehozása toofinish válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="09e61-132">toofinish creating your connection, choose **Create**.</span></span>
   
    ![integráció fiók kapcsolódási adatait.](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. <span data-ttu-id="09e61-134">Miután létrehozta a kapcsolatot, ebben a példában látható módon, válassza ki a hello X12 egybesimított fájl üzenet toodecode.</span><span class="sxs-lookup"><span data-stu-id="09e61-134">After your connection is created, as shown in this example, select hello X12 flat file message toodecode.</span></span>

    ![integráció fiók kapcsolat létrehozása](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    <span data-ttu-id="09e61-136">Példa:</span><span class="sxs-lookup"><span data-stu-id="09e61-136">For example:</span></span>

    ![Jelölje be X12 lapos fájl üzenet dekódolását](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-details"></a><span data-ttu-id="09e61-138">X12 dekódolása részletei</span><span class="sxs-lookup"><span data-stu-id="09e61-138">X12 Decode details</span></span>

<span data-ttu-id="09e61-139">hello X12 dekódolási összekötő az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="09e61-139">hello X12 Decode connector performs these tasks:</span></span>

* <span data-ttu-id="09e61-140">Kereskedelmipartner-egyezmény elleni hello boríték ellenőrzi</span><span class="sxs-lookup"><span data-stu-id="09e61-140">Validates hello envelope against trading partner agreement</span></span>
* <span data-ttu-id="09e61-141">Érvényesíti EDI- és erőforráspartner jellemző tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="09e61-141">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="09e61-142">EDI strukturális érvényesítése és a bővített sémaérvényesítése</span><span class="sxs-lookup"><span data-stu-id="09e61-142">EDI structural validation, and extended schema validation</span></span>
  * <span data-ttu-id="09e61-143">Hello interchange boríték hello szerkezete érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="09e61-143">Validation of hello structure of hello interchange envelope.</span></span>
  * <span data-ttu-id="09e61-144">A sémaérvényesítés hello boríték hello vezérlő sémának.</span><span class="sxs-lookup"><span data-stu-id="09e61-144">Schema validation of hello envelope against hello control schema.</span></span>
  * <span data-ttu-id="09e61-145">A sémaérvényesítés hello tranzakció-set adatelemek hello üzenet sémának.</span><span class="sxs-lookup"><span data-stu-id="09e61-145">Schema validation of hello transaction-set data elements against hello message schema.</span></span>
  * <span data-ttu-id="09e61-146">Tranzakció-set adatelemek végre EDI érvényesítése</span><span class="sxs-lookup"><span data-stu-id="09e61-146">EDI validation performed on transaction-set data elements</span></span> 
* <span data-ttu-id="09e61-147">Ellenőrzi, hogy hello interchange, a csoport és a tranzakció set vezérlő számok nem ismétlődő</span><span class="sxs-lookup"><span data-stu-id="09e61-147">Verifies that hello interchange, group, and transaction set control numbers are not duplicates</span></span>
  * <span data-ttu-id="09e61-148">Ellenőrzi, hello interchange ellenőrző szám korábban fogadott kereszteződéseket ellen.</span><span class="sxs-lookup"><span data-stu-id="09e61-148">Checks hello interchange control number against previously received interchanges.</span></span>
  * <span data-ttu-id="09e61-149">Hello csoport ellenőrző szám más hello interchange csoport vezérlő számoknak ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="09e61-149">Checks hello group control number against other group control numbers in hello interchange.</span></span>
  * <span data-ttu-id="09e61-150">Ellenőrzi, hogy hello tranzakció beállítása vezérlő számoknak más tranzakció set vezérlő csoport.</span><span class="sxs-lookup"><span data-stu-id="09e61-150">Checks hello transaction set control number against other transaction set control numbers in that group.</span></span>
* <span data-ttu-id="09e61-151">Felosztja a hello interchange tranzakció be, vagy megőrzi a teljes adatcsere hello:</span><span class="sxs-lookup"><span data-stu-id="09e61-151">Splits hello interchange into transaction sets, or preserves hello entire interchange:</span></span>
  * <span data-ttu-id="09e61-152">Vegyes Interchange, tranzakció készletek - tranzakció készletek felfüggeszteni hiba: elágazást interchange tranzakcióban állítja be, és minden tranzakció set elemzi.</span><span class="sxs-lookup"><span data-stu-id="09e61-152">Split Interchange as transaction sets - suspend transaction sets on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="09e61-153">hello X12 dekódolási művelet kimenetében csak tranzakció ezekről érvényesítése túl teljesítő`badMessages`, és kiírja a fennmaradó tranzakciók hello beállítása túl`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="09e61-153">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="09e61-154">Vegyes Interchange, tranzakció készletek - interchange felfüggeszteni hiba: elágazást interchange tranzakcióban állítja be, és minden tranzakció set elemzi.</span><span class="sxs-lookup"><span data-stu-id="09e61-154">Split Interchange as transaction sets - suspend interchange on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="09e61-155">Ha egy vagy több tranzakció hello interchange sikertelen érvényesítési állítja, hello X12 dekódolási művelet kimenetében összes hello tranzakció túl beállítja, hogy interchange`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="09e61-155">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span>
  * <span data-ttu-id="09e61-156">Megőrizheti a adatcsere - tranzakció készletek felfüggeszteni hiba: Preserve hello adatcsere és a folyamat hello teljes kötegelt adatcserét.</span><span class="sxs-lookup"><span data-stu-id="09e61-156">Preserve Interchange - suspend transaction sets on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="09e61-157">hello X12 dekódolási művelet kimenetében csak tranzakció ezekről érvényesítése túl teljesítő`badMessages`, és kiírja a fennmaradó tranzakciók hello beállítása túl`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="09e61-157">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="09e61-158">Megőrizheti a adatcsere - interchange felfüggeszteni hiba: Preserve hello adatcsere és a folyamat hello teljes kötegelt adatcserét.</span><span class="sxs-lookup"><span data-stu-id="09e61-158">Preserve Interchange - suspend interchange on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="09e61-159">Ha egy vagy több tranzakció hello interchange sikertelen érvényesítési állítja, hello X12 dekódolási művelet kimenetében összes hello tranzakció túl beállítja, hogy interchange`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="09e61-159">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span> 
* <span data-ttu-id="09e61-160">Létrehozza a műszaki és/vagy funkcionális visszaigazolás (Ha be van állítva).</span><span class="sxs-lookup"><span data-stu-id="09e61-160">Generates a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="09e61-161">A műszaki visszaigazolás miatt érvényesítési fejléc állít elő.</span><span class="sxs-lookup"><span data-stu-id="09e61-161">A Technical Acknowledgment generates as a result of header validation.</span></span> <span data-ttu-id="09e61-162">hello műszaki visszaigazolás jelentések hello állapota hello feldolgozási tartozó az adatcsere fejléc és hello cím címzett.</span><span class="sxs-lookup"><span data-stu-id="09e61-162">hello technical acknowledgment reports hello status of hello processing of an interchange header and trailer by hello address receiver.</span></span>
  * <span data-ttu-id="09e61-163">A működési visszaigazolás miatt törzs érvényesítési állít elő.</span><span class="sxs-lookup"><span data-stu-id="09e61-163">A Functional Acknowledgment generates as a result of body validation.</span></span> <span data-ttu-id="09e61-164">hello működési visszaigazolás jelentések minden közben hiba történt hello feldolgozása kapott dokumentum</span><span class="sxs-lookup"><span data-stu-id="09e61-164">hello functional acknowledgment reports each error encountered while processing hello received document</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="09e61-165">Nézet hello swagger</span><span class="sxs-lookup"><span data-stu-id="09e61-165">View hello swagger</span></span>
<span data-ttu-id="09e61-166">Lásd: hello [részletek swagger](/connectors/x12/).</span><span class="sxs-lookup"><span data-stu-id="09e61-166">See hello [swagger details](/connectors/x12/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="09e61-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09e61-167">Next steps</span></span>
[<span data-ttu-id="09e61-168">További tudnivalók a vállalati integrációs csomag hello</span><span class="sxs-lookup"><span data-stu-id="09e61-168">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

