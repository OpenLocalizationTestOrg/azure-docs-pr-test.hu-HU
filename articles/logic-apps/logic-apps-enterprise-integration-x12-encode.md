---
title: "aaaEncode X12 üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "EDI érvényesítése és átalakítani az XML-kódolású X12 helyű üzenet hello vállalati integrációs csomag a kódoló Azure Logic Apps"
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
ms.openlocfilehash: 785dbd2c7c82463154732921e07e3586d2307840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="7de89-103">Kódolja X12 hello vállalati integrációs csomagot az Azure Logic Apps állapotüzenete</span><span class="sxs-lookup"><span data-stu-id="7de89-103">Encode X12 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="7de89-104">Hello Encode X12 üzenet összekötővel EDI és a partner jellemző tulajdonságok ellenőrzése, XML-kódolású üzenetekkel átalakítása EDI tranzakció halmazok hello adatcsere és kérelem műszaki nyugtázási, működési nyugtázási vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="7de89-104">With hello Encode X12 message connector, you can validate EDI and partner-specific properties, convert XML-encoded messages into EDI transaction sets in hello interchange, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="7de89-105">toouse ezt az összekötőt, hozzá kell adnia a Logic Apps alkalmazást az eseményindító meglévő hello összekötő tooan.</span><span class="sxs-lookup"><span data-stu-id="7de89-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="7de89-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="7de89-106">Before you start</span></span>

<span data-ttu-id="7de89-107">Itt van szüksége hello elemeket:</span><span class="sxs-lookup"><span data-stu-id="7de89-107">Here's hello items you need:</span></span>

* <span data-ttu-id="7de89-108">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="7de89-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="7de89-109">Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="7de89-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="7de89-110">Az integráció fiók toouse hello Encode X12 üzenet csatlakozó kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7de89-110">You must have an integration account toouse hello Encode X12 message connector.</span></span>
* <span data-ttu-id="7de89-111">Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="7de89-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="7de89-112">Egy [X12 megállapodás](logic-apps-enterprise-integration-x12.md) , amely már definiálva van az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="7de89-112">An [X12 agreement](logic-apps-enterprise-integration-x12.md) that's already defined in your integration account</span></span>

## <a name="encode-x12-messages"></a><span data-ttu-id="7de89-113">Kódolja X12 üzenetek</span><span class="sxs-lookup"><span data-stu-id="7de89-113">Encode X12 messages</span></span>

1. <span data-ttu-id="7de89-114">[Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="7de89-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="7de89-115">hello Encode X12 üzenet összekötő eseményindítókat, nem rendelkezik, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például egy kérelem eseményindító.</span><span class="sxs-lookup"><span data-stu-id="7de89-115">hello Encode X12 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="7de89-116">A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7de89-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="7de89-117">Hello keresési mezőbe írja be a "x12" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="7de89-117">In hello search box, enter "x12" for your filter.</span></span> <span data-ttu-id="7de89-118">Válassza ki vagy **X12-tooX12 üzenetek kódolása szerződésnév által** vagy **X12-tooX12 üzenetek kódolása identitások által**.</span><span class="sxs-lookup"><span data-stu-id="7de89-118">Select either **X12 - Encode tooX12 message by agreement name** or **X12 - Encode tooX12 message by identities**.</span></span>
   
    ![Keressen a "x12"](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. <span data-ttu-id="7de89-120">Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="7de89-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="7de89-121">A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot.</span><span class="sxs-lookup"><span data-stu-id="7de89-121">Name your connection, and select hello integration account that you want tooconnect.</span></span> 
   
    ![integráció fiók kapcsolat](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    <span data-ttu-id="7de89-123">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="7de89-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="7de89-124">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7de89-124">Property</span></span> | <span data-ttu-id="7de89-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="7de89-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="7de89-126">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="7de89-126">Connection Name *</span></span> |<span data-ttu-id="7de89-127">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="7de89-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="7de89-128">Integráció fiók *</span><span class="sxs-lookup"><span data-stu-id="7de89-128">Integration Account *</span></span> |<span data-ttu-id="7de89-129">Adja meg a integrációs fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="7de89-129">Enter a name for your integration account.</span></span> <span data-ttu-id="7de89-130">Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="7de89-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="7de89-131">Amikor elkészült, a kapcsolat adatai alábbihoz hasonló toothis példa.</span><span class="sxs-lookup"><span data-stu-id="7de89-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="7de89-132">a kapcsolat létrehozása toofinish válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7de89-132">toofinish creating your connection, choose **Create**.</span></span>

    ![integráció fiók kapcsolat létrehozása](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    <span data-ttu-id="7de89-134">A kapcsolat létrejön.</span><span class="sxs-lookup"><span data-stu-id="7de89-134">Your connection is now created.</span></span>

    ![integráció fiók kapcsolódási adatait.](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a><span data-ttu-id="7de89-136">Kódolja X12 szerződés neve üzeneteket</span><span class="sxs-lookup"><span data-stu-id="7de89-136">Encode X12 messages by agreement name</span></span>

<span data-ttu-id="7de89-137">Ha úgy dönt, tooencode X12 üzenetek szerződés neve, nyissa meg a hello **X12 neve megállapodás** listában, adja meg vagy válassza ki a meglévő X12 szerződést.</span><span class="sxs-lookup"><span data-stu-id="7de89-137">If you chose tooencode X12 messages by agreement name, open hello **Name of X12 agreement** list, enter or select your existing X12 agreement.</span></span> <span data-ttu-id="7de89-138">Adja meg a hello XML üzenet tooencode.</span><span class="sxs-lookup"><span data-stu-id="7de89-138">Enter hello XML message tooencode.</span></span>

![Adja meg a X12 szerződés neve és az XML-üzenet tooencode](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a><span data-ttu-id="7de89-140">Kódolja X12 identitások üzenetek</span><span class="sxs-lookup"><span data-stu-id="7de89-140">Encode X12 messages by identities</span></span>

<span data-ttu-id="7de89-141">Ha tooencode X12 üzenetek által identitások választja, adja meg hello a küldő azonosítója, a küldő minősítő, a fogadó azonosítója és a fogadó minősítő a X12 a szerződést.</span><span class="sxs-lookup"><span data-stu-id="7de89-141">If you choose tooencode X12 messages by identities, enter hello sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your X12 agreement.</span></span> <span data-ttu-id="7de89-142">Válassza ki a hello XML üzenet tooencode.</span><span class="sxs-lookup"><span data-stu-id="7de89-142">Select hello XML message tooencode.</span></span>
   
![Adjon meg a küldő és fogadó identitásainak, válassza az XML-üzenet tooencode](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a><span data-ttu-id="7de89-144">X12 részletek kódolása</span><span class="sxs-lookup"><span data-stu-id="7de89-144">X12 Encode details</span></span>

<span data-ttu-id="7de89-145">hello X12 Encode összekötő az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="7de89-145">hello X12 Encode connector performs these tasks:</span></span>

* <span data-ttu-id="7de89-146">A szerződés feloldási összekapcsolja a küldő és fogadó környezeti tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="7de89-146">Agreement resolution by matching sender and receiver context properties.</span></span>
* <span data-ttu-id="7de89-147">XML-kódolású üzenetekkel konvertálásakor EDI tranzakció halmazok hello interchange hello EDI-interchange rendezi sorba.</span><span class="sxs-lookup"><span data-stu-id="7de89-147">Serializes hello EDI interchange, converting XML-encoded messages into EDI transaction sets in hello interchange.</span></span>
* <span data-ttu-id="7de89-148">Tranzakció set fejléc és pótkocsi szegmensek vonatkozik</span><span class="sxs-lookup"><span data-stu-id="7de89-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="7de89-149">Az adatcsere ellenőrző szám, a vezérlő csoportszámmal és a tranzakció beállítása vezérlő száma minden kimenő adatcserét hoz létre</span><span class="sxs-lookup"><span data-stu-id="7de89-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="7de89-150">A felváltja elválasztók hello hasznos adatban</span><span class="sxs-lookup"><span data-stu-id="7de89-150">Replaces separators in hello payload data</span></span>
* <span data-ttu-id="7de89-151">Érvényesíti EDI- és erőforráspartner jellemző tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="7de89-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="7de89-152">A sémaérvényesítés hello tranzakció-set adatelemek üdvözlőüzenetére séma ellen</span><span class="sxs-lookup"><span data-stu-id="7de89-152">Schema validation of hello transaction-set data elements against hello message Schema</span></span>
  * <span data-ttu-id="7de89-153">EDI érvényesítési tranzakció-set adatelemek végre.</span><span class="sxs-lookup"><span data-stu-id="7de89-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="7de89-154">Tranzakció-set adatelemek végre kiterjesztett érvényesítése</span><span class="sxs-lookup"><span data-stu-id="7de89-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="7de89-155">A műszaki és/vagy funkcionális visszaigazolás-kérelmek (Ha be van állítva).</span><span class="sxs-lookup"><span data-stu-id="7de89-155">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="7de89-156">A műszaki visszaigazolás miatt érvényesítési fejléc állít elő.</span><span class="sxs-lookup"><span data-stu-id="7de89-156">A Technical Acknowledgment generates as a result of header validation.</span></span> <span data-ttu-id="7de89-157">hello műszaki visszaigazolás jelentések hello állapot hello feldolgozási tartozó az adatcsere fejléc és hello cím címzett</span><span class="sxs-lookup"><span data-stu-id="7de89-157">hello technical acknowledgment reports hello status of hello processing of an interchange header and trailer by hello address receiver</span></span>
  * <span data-ttu-id="7de89-158">A működési visszaigazolás miatt törzs érvényesítési állít elő.</span><span class="sxs-lookup"><span data-stu-id="7de89-158">A Functional Acknowledgment generates as a result of body validation.</span></span> <span data-ttu-id="7de89-159">hello működési visszaigazolás jelentések minden közben hiba történt hello feldolgozása kapott dokumentum</span><span class="sxs-lookup"><span data-stu-id="7de89-159">hello functional acknowledgment reports each error encountered while processing hello received document</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="7de89-160">Nézet hello swagger</span><span class="sxs-lookup"><span data-stu-id="7de89-160">View hello swagger</span></span>
<span data-ttu-id="7de89-161">Lásd: hello [részletek swagger](/connectors/x12/).</span><span class="sxs-lookup"><span data-stu-id="7de89-161">See hello [swagger details](/connectors/x12/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7de89-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7de89-162">Next steps</span></span>
[<span data-ttu-id="7de89-163">További tudnivalók a vállalati integrációs csomag hello</span><span class="sxs-lookup"><span data-stu-id="7de89-163">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

