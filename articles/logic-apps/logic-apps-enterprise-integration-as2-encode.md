---
title: "aaaEncode AS2 üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "Hogyan toouse hello AS2 kódoló hello vállalati integrációs csomagot az Azure Logic Apps"
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
ms.openlocfilehash: 2b372c416512ffa9ea5dc50ce0f767bfd8aefbc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="18af9-103">AS2 üzenetek kódolása az Azure Logic Apps a hello vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="18af9-103">Encode AS2 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="18af9-104">tooestablish biztonsága és megbízhatósága üzenetek átvitele közben hello kódolása AS2 üzenet összekötő használatára.</span><span class="sxs-lookup"><span data-stu-id="18af9-104">tooestablish security and reliability while transmitting messages, use hello Encode AS2 message connector.</span></span> <span data-ttu-id="18af9-105">Ez az összekötő digitális aláírására, a titkosítás és a nyugtázásokhoz keresztül üzenet törlése értesítések (MDN), ami is toosupport a Megtagadás nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="18af9-105">This connector provides digital signing, encryption, and acknowledgements through Message Disposition Notifications (MDN), which also leads toosupport for Non-Repudiation.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="18af9-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="18af9-106">Before you start</span></span>

<span data-ttu-id="18af9-107">Itt van szüksége hello elemeket:</span><span class="sxs-lookup"><span data-stu-id="18af9-107">Here's hello items you need:</span></span>

* <span data-ttu-id="18af9-108">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="18af9-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="18af9-109">Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="18af9-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="18af9-110">Az integráció fiók toouse hello kódolása AS2 üzenet csatlakozó kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="18af9-110">You must have an integration account toouse hello Encode AS2 message connector.</span></span>
* <span data-ttu-id="18af9-111">Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="18af9-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="18af9-112">Egy [AS2 megállapodás](logic-apps-enterprise-integration-as2.md) , amely már definiálva van az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="18af9-112">An [AS2 agreement](logic-apps-enterprise-integration-as2.md) that's already defined in your integration account</span></span>

## <a name="encode-as2-messages"></a><span data-ttu-id="18af9-113">AS2-üzenetek kódolása</span><span class="sxs-lookup"><span data-stu-id="18af9-113">Encode AS2 messages</span></span>

1. <span data-ttu-id="18af9-114">[Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="18af9-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="18af9-115">hello kódolása AS2-üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="18af9-115">hello Encode AS2 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="18af9-116">A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="18af9-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="18af9-117">Hello keresési mezőbe írja be a "AS2" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="18af9-117">In hello search box, enter "AS2" for your filter.</span></span> <span data-ttu-id="18af9-118">Válassza ki **AS2 - Encode AS2 üzenet**.</span><span class="sxs-lookup"><span data-stu-id="18af9-118">Select **AS2 - Encode AS2 message**.</span></span>
   
    ![Keresse meg a "AS2"](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. <span data-ttu-id="18af9-120">Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="18af9-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="18af9-121">A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot.</span><span class="sxs-lookup"><span data-stu-id="18af9-121">Name your connection, and select hello integration account that you want tooconnect.</span></span> 
   
    ![kapcsolat toointegration-fiók létrehozása](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    <span data-ttu-id="18af9-123">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="18af9-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="18af9-124">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="18af9-124">Property</span></span> | <span data-ttu-id="18af9-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="18af9-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="18af9-126">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="18af9-126">Connection Name *</span></span> |<span data-ttu-id="18af9-127">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="18af9-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="18af9-128">Integráció fiók *</span><span class="sxs-lookup"><span data-stu-id="18af9-128">Integration Account *</span></span> |<span data-ttu-id="18af9-129">Adja meg a integrációs fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="18af9-129">Enter a name for your integration account.</span></span> <span data-ttu-id="18af9-130">Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="18af9-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="18af9-131">Amikor elkészült, a kapcsolat adatai alábbihoz hasonló toothis példa.</span><span class="sxs-lookup"><span data-stu-id="18af9-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="18af9-132">a kapcsolat létrehozása toofinish válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="18af9-132">toofinish creating your connection, choose **Create**.</span></span>
   
    ![integráció kapcsolódási adatait.](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. <span data-ttu-id="18af9-134">Miután létrehozta a kapcsolatot, ebben a példában látható módon, adja meg a részletes **AS2-a**, **AS2-tooidentifiers** be a szerződés és **törzs**, amely hello üzenetadatokat.</span><span class="sxs-lookup"><span data-stu-id="18af9-134">After your connection is created, as shown in this example, provide details for **AS2-From**, **AS2-tooidentifiers** as configured in your agreement, and **Body**, which is hello message payload.</span></span>
   
    ![Adja meg a kötelező mezők](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a><span data-ttu-id="18af9-136">AS2-kódoló részletei</span><span class="sxs-lookup"><span data-stu-id="18af9-136">AS2 encoder details</span></span>

<span data-ttu-id="18af9-137">hello kódolása AS2-összekötő az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="18af9-137">hello Encode AS2 connector performs these tasks:</span></span> 

* <span data-ttu-id="18af9-138">AS2/HTTP-fejlécek vonatkozik</span><span class="sxs-lookup"><span data-stu-id="18af9-138">Applies AS2/HTTP headers</span></span>
* <span data-ttu-id="18af9-139">A jelek kimenő üzenetek (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="18af9-139">Signs outgoing messages (if configured)</span></span>
* <span data-ttu-id="18af9-140">Kimenő üzenetek titkosítja (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="18af9-140">Encrypts outgoing messages (if configured)</span></span>
* <span data-ttu-id="18af9-141">Tömöríti a üdvözlőüzenetére (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="18af9-141">Compresses hello message (if configured)</span></span>

## <a name="try-this-sample"></a><span data-ttu-id="18af9-142">Ismételje meg ezt a mintát</span><span class="sxs-lookup"><span data-stu-id="18af9-142">Try this sample</span></span>

<span data-ttu-id="18af9-143">tootry forgatókönyv üzembe helyezését egy teljesen működőképes logic app és minta AS2, lásd: hello [AS2 logic app-sablon és a forgatókönyv](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span><span class="sxs-lookup"><span data-stu-id="18af9-143">tootry deploying a fully operational logic app and sample AS2 scenario, see hello [AS2 logic app template and scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="18af9-144">Nézet hello swagger</span><span class="sxs-lookup"><span data-stu-id="18af9-144">View hello swagger</span></span>
<span data-ttu-id="18af9-145">Lásd: hello [részletek swagger](/connectors/as2/).</span><span class="sxs-lookup"><span data-stu-id="18af9-145">See hello [swagger details](/connectors/as2/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="18af9-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18af9-146">Next steps</span></span>
[<span data-ttu-id="18af9-147">További tudnivalók a vállalati integrációs csomag hello</span><span class="sxs-lookup"><span data-stu-id="18af9-147">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

