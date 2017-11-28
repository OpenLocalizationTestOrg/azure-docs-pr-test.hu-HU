---
title: "AS2-üzenetek - Azure Logic Apps kódolása |} Microsoft Docs"
description: "Az AS2 kódoló a vállalati integrációs csomagot az Azure Logic Apps használata"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 7889bf9e4e02143b6bb4c797531afa54f8647ce5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="11d26-103">Az Azure Logic Apps a vállalati integrációs csomaggal AS2 üzenetek kódolása</span><span class="sxs-lookup"><span data-stu-id="11d26-103">Encode AS2 messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="11d26-104">Biztonsága és megbízhatósága közben üzenetek továbbítására létrehozásához az AS2 kódolása üzenet összekötő használatára.</span><span class="sxs-lookup"><span data-stu-id="11d26-104">To establish security and reliability while transmitting messages, use the Encode AS2 message connector.</span></span> <span data-ttu-id="11d26-105">Ez az összekötő biztosít digitális aláírására, a titkosítás és a nyugtázásokhoz keresztül üzenet törlése értesítések (MDN), amely még nem megtagadás támogatása.</span><span class="sxs-lookup"><span data-stu-id="11d26-105">This connector provides digital signing, encryption, and acknowledgements through Message Disposition Notifications (MDN), which also leads to support for Non-Repudiation.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="11d26-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="11d26-106">Before you start</span></span>

<span data-ttu-id="11d26-107">A szükséges elemeket itt található:</span><span class="sxs-lookup"><span data-stu-id="11d26-107">Here's the items you need:</span></span>

* <span data-ttu-id="11d26-108">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="11d26-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="11d26-109">Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="11d26-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="11d26-110">Az AS2 kódolása üzenet összekötő használatára integrációs fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="11d26-110">You must have an integration account to use the Encode AS2 message connector.</span></span>
* <span data-ttu-id="11d26-111">Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="11d26-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="11d26-112">Egy [AS2 megállapodás](logic-apps-enterprise-integration-as2.md) , amely már definiálva van az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="11d26-112">An [AS2 agreement](logic-apps-enterprise-integration-as2.md) that's already defined in your integration account</span></span>

## <a name="encode-as2-messages"></a><span data-ttu-id="11d26-113">AS2-üzenetek kódolása</span><span class="sxs-lookup"><span data-stu-id="11d26-113">Encode AS2 messages</span></span>

1. <span data-ttu-id="11d26-114">[Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="11d26-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="11d26-115">Az AS2 kódolása üzenet összekötő eseményindítókat, nem rendelkezik, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például egy kérelem eseményindító.</span><span class="sxs-lookup"><span data-stu-id="11d26-115">The Encode AS2 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="11d26-116">A Logic App-tervezőben, vegye fel egy eseményindítót, majd egy műveletet a logikai alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="11d26-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="11d26-117">A keresési mezőbe írja be a "AS2" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="11d26-117">In the search box, enter "AS2" for your filter.</span></span> <span data-ttu-id="11d26-118">Válassza ki **AS2 - Encode AS2 üzenet**.</span><span class="sxs-lookup"><span data-stu-id="11d26-118">Select **AS2 - Encode AS2 message**.</span></span>
   
    ![Keresse meg a "AS2"](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. <span data-ttu-id="11d26-120">Integráció fiókjába korábban a kapcsolatokat nem hozott létre, ha a program kéri, most, hogy a kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="11d26-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="11d26-121">A kapcsolat neve, és válassza ki a integrációs fiókot, amely csatlakozni szeretne.</span><span class="sxs-lookup"><span data-stu-id="11d26-121">Name your connection, and select the integration account that you want to connect.</span></span> 
   
    ![integráció fiókhoz kapcsolat létrehozása](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    <span data-ttu-id="11d26-123">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="11d26-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="11d26-124">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="11d26-124">Property</span></span> | <span data-ttu-id="11d26-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="11d26-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="11d26-126">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="11d26-126">Connection Name *</span></span> |<span data-ttu-id="11d26-127">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="11d26-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="11d26-128">Integráció fiók *</span><span class="sxs-lookup"><span data-stu-id="11d26-128">Integration Account *</span></span> |<span data-ttu-id="11d26-129">Adja meg a integrációs fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="11d26-129">Enter a name for your integration account.</span></span> <span data-ttu-id="11d26-130">Ellenőrizze, hogy az integrációs fiók és a logikai alkalmazást az Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="11d26-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="11d26-131">Amikor elkészült, a kapcsolódási adatait. Ez a példa hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="11d26-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="11d26-132">Válassza ki a kapcsolat létrehozásának befejezéséhez **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="11d26-132">To finish creating your connection, choose **Create**.</span></span>
   
    ![integráció kapcsolódási adatait.](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. <span data-ttu-id="11d26-134">A kapcsolat létrehozása után ebben a példában látható módon, adja meg az adatokat **AS2-a**, **AS2-azonosítók a** be a szerződés és **törzs**, vagyis a üzenetadatokat.</span><span class="sxs-lookup"><span data-stu-id="11d26-134">After your connection is created, as shown in this example, provide details for **AS2-From**, **AS2-To identifiers** as configured in your agreement, and **Body**, which is the message payload.</span></span>
   
    ![Adja meg a kötelező mezők](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a><span data-ttu-id="11d26-136">AS2-kódoló részletei</span><span class="sxs-lookup"><span data-stu-id="11d26-136">AS2 encoder details</span></span>

<span data-ttu-id="11d26-137">A kódolási AS2-összekötő az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="11d26-137">The Encode AS2 connector performs these tasks:</span></span> 

* <span data-ttu-id="11d26-138">AS2/HTTP-fejlécek vonatkozik</span><span class="sxs-lookup"><span data-stu-id="11d26-138">Applies AS2/HTTP headers</span></span>
* <span data-ttu-id="11d26-139">A jelek kimenő üzenetek (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="11d26-139">Signs outgoing messages (if configured)</span></span>
* <span data-ttu-id="11d26-140">Kimenő üzenetek titkosítja (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="11d26-140">Encrypts outgoing messages (if configured)</span></span>
* <span data-ttu-id="11d26-141">Az üzenet tömöríti (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="11d26-141">Compresses the message (if configured)</span></span>

## <a name="try-this-sample"></a><span data-ttu-id="11d26-142">Ismételje meg ezt a mintát</span><span class="sxs-lookup"><span data-stu-id="11d26-142">Try this sample</span></span>

<span data-ttu-id="11d26-143">Próbálja meg egy teljesen működőképes logic app és a minta AS2 forgatókönyv üzembe helyezését, tekintse meg a [AS2 logic app-sablon és a forgatókönyv](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span><span class="sxs-lookup"><span data-stu-id="11d26-143">To try deploying a fully operational logic app and sample AS2 scenario, see the [AS2 logic app template and scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="11d26-144">A swagger megtekintése</span><span class="sxs-lookup"><span data-stu-id="11d26-144">View the swagger</span></span>
<span data-ttu-id="11d26-145">Tekintse meg a [részletek swagger](/connectors/as2/).</span><span class="sxs-lookup"><span data-stu-id="11d26-145">See the [swagger details](/connectors/as2/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="11d26-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="11d26-146">Next steps</span></span>
[<span data-ttu-id="11d26-147">További tudnivalók a vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="11d26-147">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

