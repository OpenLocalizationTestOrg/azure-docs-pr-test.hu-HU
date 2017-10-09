---
title: "aaaEncode EDIFACT üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "EDI érvényesítése és EDIFACT üzenet Encoder hello vállalati integrációs csomagot az XML létrehozása az Azure Logic Apps"
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
ms.openlocfilehash: b3799dbd2492adef597022d017cf28f5ceb1085c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="f594c-103">EDIFACT üzenetek kódolása az Azure Logic Apps a hello vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="f594c-103">Encode EDIFACT messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="f594c-104">Hello kódolása EDIFACT-üzenet összekötővel EDI és a partner jellemző tulajdonságok ellenőrzése, az XML-dokumentum az egyes tranzakció készítése és műszaki nyugtázási vagy működési visszaigazolás kérése.</span><span class="sxs-lookup"><span data-stu-id="f594c-104">With hello Encode EDIFACT message connector, you can validate EDI and partner-specific properties, generate an XML document for each transaction set, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="f594c-105">toouse ezt az összekötőt, hozzá kell adnia a Logic Apps alkalmazást az eseményindító meglévő hello összekötő tooan.</span><span class="sxs-lookup"><span data-stu-id="f594c-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="f594c-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="f594c-106">Before you start</span></span>

<span data-ttu-id="f594c-107">Itt van szüksége hello elemeket:</span><span class="sxs-lookup"><span data-stu-id="f594c-107">Here's hello items you need:</span></span>

* <span data-ttu-id="f594c-108">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="f594c-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="f594c-109">Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="f594c-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="f594c-110">Az integráció fiók toouse hello kódolása EDIFACT üzenet csatlakozó kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f594c-110">You must have an integration account toouse hello Encode EDIFACT message connector.</span></span> 
* <span data-ttu-id="f594c-111">Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="f594c-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="f594c-112">Egy [EDIFACT megállapodás](logic-apps-enterprise-integration-edifact.md) , amely már definiálva van az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="f594c-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="encode-edifact-messages"></a><span data-ttu-id="f594c-113">EDIFACT üzenetek kódolása</span><span class="sxs-lookup"><span data-stu-id="f594c-113">Encode EDIFACT messages</span></span>

1. <span data-ttu-id="f594c-114">[Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="f594c-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="f594c-115">hello kódolása EDIFACT üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="f594c-115">hello Encode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="f594c-116">A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f594c-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="f594c-117">Hello keresési mezőbe írja be a "EDIFACT" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="f594c-117">In hello search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="f594c-118">Válassza **EDIFACT üzenetből kódolása szerződésnév** vagy **Encode tooEDIFACT üzenetre identitások**.</span><span class="sxs-lookup"><span data-stu-id="f594c-118">Select either **Encode EDIFACT Message by agreement name** or **Encode tooEDIFACT message by identities**.</span></span>
   
    ![EDIFACT keresése](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. <span data-ttu-id="f594c-120">Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="f594c-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="f594c-121">A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot.</span><span class="sxs-lookup"><span data-stu-id="f594c-121">Name your connection, and select hello integration account that you want tooconnect.</span></span>

    ![integráció fiók kapcsolat létrehozása](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    <span data-ttu-id="f594c-123">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="f594c-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="f594c-124">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f594c-124">Property</span></span> | <span data-ttu-id="f594c-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="f594c-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="f594c-126">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="f594c-126">Connection Name *</span></span> |<span data-ttu-id="f594c-127">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="f594c-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="f594c-128">Integráció fiók *</span><span class="sxs-lookup"><span data-stu-id="f594c-128">Integration Account *</span></span> |<span data-ttu-id="f594c-129">Adja meg a integrációs fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="f594c-129">Enter a name for your integration account.</span></span> <span data-ttu-id="f594c-130">Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="f594c-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="f594c-131">Amikor elkészült, a kapcsolat adatai alábbihoz hasonló toothis példa.</span><span class="sxs-lookup"><span data-stu-id="f594c-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="f594c-132">a kapcsolat létrehozása toofinish válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f594c-132">toofinish creating your connection, choose **Create**.</span></span>

    ![integráció fiók kapcsolódási adatait.](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    <span data-ttu-id="f594c-134">A kapcsolat létrejön.</span><span class="sxs-lookup"><span data-stu-id="f594c-134">Your connection is now created.</span></span>

    ![integráció fiók kapcsolat létrehozása](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a><span data-ttu-id="f594c-136">EDIFACT üzenetek kódolása szerződés neve</span><span class="sxs-lookup"><span data-stu-id="f594c-136">Encode EDIFACT Message by agreement name</span></span>

<span data-ttu-id="f594c-137">Ha úgy dönt, tooencode EDIFACT üzenetek szerződés neve, nyissa meg a hello **nevét a EDIFACT megállapodás** listában, adja meg vagy válassza ki a EDIFACT a szerződés nevét.</span><span class="sxs-lookup"><span data-stu-id="f594c-137">If you chose tooencode EDIFACT messages by agreement name, open hello **Name of EDIFACT agreement** list, enter or select your EDIFACT agreement name.</span></span> <span data-ttu-id="f594c-138">Adja meg a hello XML üzenet tooencode.</span><span class="sxs-lookup"><span data-stu-id="f594c-138">Enter hello XML message tooencode.</span></span>

![Adja meg a EDIFACT szerződés neve és XML üzenet tooencode](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a><span data-ttu-id="f594c-140">EDIFACT üzenetek által identitások kódolása</span><span class="sxs-lookup"><span data-stu-id="f594c-140">Encode EDIFACT Message by identities</span></span>

<span data-ttu-id="f594c-141">Ha tooencode EDIFACT üzenetek által identitások, adja meg a hello a küldő azonosítója, a küldő minősítő, a fogadó azonosítója és a fogadó minősítő EDIFACT kötött konfigurált módon.</span><span class="sxs-lookup"><span data-stu-id="f594c-141">If you choose tooencode EDIFACT messages by identities, enter hello sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your EDIFACT agreement.</span></span> <span data-ttu-id="f594c-142">Válassza ki a hello XML üzenet tooencode.</span><span class="sxs-lookup"><span data-stu-id="f594c-142">Select hello XML message tooencode.</span></span>

![Adjon meg a küldő és fogadó identitásainak, válassza az XML-üzenet tooencode](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a><span data-ttu-id="f594c-144">EDIFACT kódolása részletei</span><span class="sxs-lookup"><span data-stu-id="f594c-144">EDIFACT Encode details</span></span>

<span data-ttu-id="f594c-145">hello kódolása EDIFACT-összekötő az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="f594c-145">hello Encode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="f594c-146">Hello megállapodás oldani az egyező hello küldő minősítő & azonosítója és a fogadó minősítő és azonosítója</span><span class="sxs-lookup"><span data-stu-id="f594c-146">Resolve hello agreement by matching hello sender qualifier & identifier and receiver qualifier and identifier</span></span>
* <span data-ttu-id="f594c-147">XML-kódolású üzenetekkel konvertálásakor EDI tranzakció halmazok hello interchange hello EDI-interchange rendezi sorba.</span><span class="sxs-lookup"><span data-stu-id="f594c-147">Serializes hello EDI interchange, converting XML-encoded messages into EDI transaction sets in hello interchange.</span></span>
* <span data-ttu-id="f594c-148">Tranzakció set fejléc és pótkocsi szegmensek vonatkozik</span><span class="sxs-lookup"><span data-stu-id="f594c-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="f594c-149">Az adatcsere ellenőrző szám, a vezérlő csoportszámmal és a tranzakció beállítása vezérlő száma minden kimenő adatcserét hoz létre</span><span class="sxs-lookup"><span data-stu-id="f594c-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="f594c-150">A felváltja elválasztók hello hasznos adatban</span><span class="sxs-lookup"><span data-stu-id="f594c-150">Replaces separators in hello payload data</span></span>
* <span data-ttu-id="f594c-151">Érvényesíti EDI- és erőforráspartner jellemző tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="f594c-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="f594c-152">A sémaérvényesítés hello tranzakció-set adatelemek hello üzenet sémának.</span><span class="sxs-lookup"><span data-stu-id="f594c-152">Schema validation of hello transaction-set data elements against hello message schema.</span></span>
  * <span data-ttu-id="f594c-153">EDI érvényesítési tranzakció-set adatelemek végre.</span><span class="sxs-lookup"><span data-stu-id="f594c-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="f594c-154">Tranzakció-set adatelemek végre kiterjesztett érvényesítése</span><span class="sxs-lookup"><span data-stu-id="f594c-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="f594c-155">Az XML-dokumentum az egyes tranzakció állít elő.</span><span class="sxs-lookup"><span data-stu-id="f594c-155">Generates an XML document for each transaction set.</span></span>
* <span data-ttu-id="f594c-156">A műszaki és/vagy funkcionális visszaigazolás-kérelmek (Ha be van állítva).</span><span class="sxs-lookup"><span data-stu-id="f594c-156">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="f594c-157">A műszaki visszaigazolás, mint hello CONTRL üzenet azt jelzi, az cseréje fogadását.</span><span class="sxs-lookup"><span data-stu-id="f594c-157">As a technical acknowledgment, hello CONTRL message indicates receipt of an interchange.</span></span>
  * <span data-ttu-id="f594c-158">A működési visszaigazolás, mint hello CONTRL az üzenet azt jelzi, elfogadási vagy a kapott hello interchange, vagy az üzenet, a hibák vagy nem támogatott funkciók listáját elutasítása</span><span class="sxs-lookup"><span data-stu-id="f594c-158">As a functional acknowledgment, hello CONTRL message indicates acceptance or rejection of hello received interchange, group, or message, with a list of errors or unsupported functionality</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="f594c-159">A Swagger-fájl megtekintése</span><span class="sxs-lookup"><span data-stu-id="f594c-159">View Swagger file</span></span>
<span data-ttu-id="f594c-160">tooview hello Swagger részletek hello EDIFACT-összekötőhöz: [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="f594c-160">tooview hello Swagger details for hello EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f594c-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f594c-161">Next steps</span></span>
[<span data-ttu-id="f594c-162">További tudnivalók a vállalati integrációs csomag hello</span><span class="sxs-lookup"><span data-stu-id="f594c-162">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

