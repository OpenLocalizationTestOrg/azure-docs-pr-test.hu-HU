---
title: "aaaDecode AS2 üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "Hogyan toouse hello AS2 dekóder hello vállalati integrációs csomagot az Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 2406e5ec68e0906700fad97d60cb83ef0d106cd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="2d470-103">AS2-üzenetek dekódolási az Azure Logic Apps a hello vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="2d470-103">Decode AS2 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span> 

<span data-ttu-id="2d470-104">tooestablish biztonsága és megbízhatósága üzenetek átvitele közben hello dekódolása AS2 üzenet összekötő használatára.</span><span class="sxs-lookup"><span data-stu-id="2d470-104">tooestablish security and reliability while transmitting messages, use hello Decode AS2 message connector.</span></span> <span data-ttu-id="2d470-105">Ez az összekötő biztosít digitális aláírására, visszafejtéshez és nyugták üzenet törlése értesítések (MDN) keresztül.</span><span class="sxs-lookup"><span data-stu-id="2d470-105">This connector provides digital signing, decryption, and acknowledgements through Message Disposition Notifications (MDN).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="2d470-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2d470-106">Before you start</span></span>

<span data-ttu-id="2d470-107">Itt van szüksége hello elemeket:</span><span class="sxs-lookup"><span data-stu-id="2d470-107">Here's hello items you need:</span></span>

* <span data-ttu-id="2d470-108">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="2d470-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="2d470-109">Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="2d470-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="2d470-110">Az integráció fiók toouse hello dekódolása AS2 üzenet csatlakozó kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2d470-110">You must have an integration account toouse hello Decode AS2 message connector.</span></span>
* <span data-ttu-id="2d470-111">Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="2d470-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="2d470-112">Egy [AS2 megállapodás](logic-apps-enterprise-integration-as2.md) , amely már definiálva van az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="2d470-112">An [AS2 agreement](logic-apps-enterprise-integration-as2.md) that's already defined in your integration account</span></span>

## <a name="decode-as2-messages"></a><span data-ttu-id="2d470-113">AS2-üzenetek dekódolása</span><span class="sxs-lookup"><span data-stu-id="2d470-113">Decode AS2 messages</span></span>

1. <span data-ttu-id="2d470-114">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="2d470-114">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="2d470-115">hello dekódolása AS2-üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="2d470-115">hello Decode AS2 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="2d470-116">A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2d470-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="2d470-117">Hello keresési mezőbe írja be a "AS2" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="2d470-117">In hello search box, enter "AS2" for your filter.</span></span> <span data-ttu-id="2d470-118">Válassza ki **AS2 - dekódolási AS2 üzenet**.</span><span class="sxs-lookup"><span data-stu-id="2d470-118">Select **AS2 - Decode AS2 message**.</span></span>
   
    ![Keresse meg a "AS2"](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. <span data-ttu-id="2d470-120">Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="2d470-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="2d470-121">A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot.</span><span class="sxs-lookup"><span data-stu-id="2d470-121">Name your connection, and select hello integration account that you want tooconnect.</span></span>
   
    ![Integráció kapcsolat létrehozása](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    <span data-ttu-id="2d470-123">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="2d470-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="2d470-124">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2d470-124">Property</span></span> | <span data-ttu-id="2d470-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="2d470-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="2d470-126">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="2d470-126">Connection Name *</span></span> |<span data-ttu-id="2d470-127">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="2d470-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="2d470-128">Integráció fiók *</span><span class="sxs-lookup"><span data-stu-id="2d470-128">Integration Account *</span></span> |<span data-ttu-id="2d470-129">Adja meg a integrációs fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="2d470-129">Enter a name for your integration account.</span></span> <span data-ttu-id="2d470-130">Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="2d470-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="2d470-131">Amikor elkészült, a kapcsolat adatai alábbihoz hasonló toothis példa.</span><span class="sxs-lookup"><span data-stu-id="2d470-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="2d470-132">a kapcsolat létrehozása toofinish válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2d470-132">toofinish creating your connection, choose **Create**.</span></span>

    ![integráció kapcsolódási adatait.](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. <span data-ttu-id="2d470-134">A kapcsolat létrehozása után ebben a példában látható módon, válassza ki a **törzs** és **fejlécek** hello a kérelem kimenetek.</span><span class="sxs-lookup"><span data-stu-id="2d470-134">After your connection is created, as shown in this example, select **Body** and **Headers** from hello Request outputs.</span></span>
   
    ![integráció kapcsolat létrehozása](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    <span data-ttu-id="2d470-136">Példa:</span><span class="sxs-lookup"><span data-stu-id="2d470-136">For example:</span></span>

    ![Válassza ki a szervezet és a fejlécek a kérelem kimenetek](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a><span data-ttu-id="2d470-138">AS2 dekóder részletei</span><span class="sxs-lookup"><span data-stu-id="2d470-138">AS2 decoder details</span></span>

<span data-ttu-id="2d470-139">hello dekódolása AS2-összekötő az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="2d470-139">hello Decode AS2 connector performs these tasks:</span></span> 

* <span data-ttu-id="2d470-140">Feldolgozza a AS2/HTTP-fejlécek</span><span class="sxs-lookup"><span data-stu-id="2d470-140">Processes AS2/HTTP headers</span></span>
* <span data-ttu-id="2d470-141">Ellenőrzi a hello aláírás (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="2d470-141">Verifies hello signature (if configured)</span></span>
* <span data-ttu-id="2d470-142">Visszafejti köszönőüzenetei (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="2d470-142">Decrypts hello messages (if configured)</span></span>
* <span data-ttu-id="2d470-143">Kibontja a üdvözlőüzenetére (Ha be van állítva)</span><span class="sxs-lookup"><span data-stu-id="2d470-143">Decompresses hello message (if configured)</span></span>
* <span data-ttu-id="2d470-144">Egyezteti a fogadott MDN hello eredeti kimenő üzenet</span><span class="sxs-lookup"><span data-stu-id="2d470-144">Reconciles a received MDN with hello original outbound message</span></span>
* <span data-ttu-id="2d470-145">Frissíti, és korrelálja hello nem megtagadás adatbázis</span><span class="sxs-lookup"><span data-stu-id="2d470-145">Updates and correlates records in hello non-repudiation database</span></span>
* <span data-ttu-id="2d470-146">Írja a rekordokat az AS2 állapotjelentés</span><span class="sxs-lookup"><span data-stu-id="2d470-146">Writes records for AS2 status reporting</span></span>
* <span data-ttu-id="2d470-147">hello kimeneti hasznos tartalma base64-kódolású</span><span class="sxs-lookup"><span data-stu-id="2d470-147">hello output payload contents are base64 encoded</span></span>
* <span data-ttu-id="2d470-148">Beállítja, hogy egy MDN szükség, hogy hello MDN kell lennie a szinkron vagy aszinkron konfigurációtól függően az AS2 egyezményben</span><span class="sxs-lookup"><span data-stu-id="2d470-148">Determines whether an MDN is required, and whether hello MDN should be synchronous or asynchronous based on configuration in AS2 agreement</span></span>
* <span data-ttu-id="2d470-149">Létrehoz egy szinkron vagy aszinkron MDN (megállapodás konfigurációk alapján)</span><span class="sxs-lookup"><span data-stu-id="2d470-149">Generates a synchronous or asynchronous MDN (based on agreement configurations)</span></span>
* <span data-ttu-id="2d470-150">Hello MDN hello korrelációs jogkivonat és a tulajdonságok beállítása</span><span class="sxs-lookup"><span data-stu-id="2d470-150">Sets hello correlation tokens and properties on hello MDN</span></span>

## <a name="try-this-sample"></a><span data-ttu-id="2d470-151">Ismételje meg ezt a mintát</span><span class="sxs-lookup"><span data-stu-id="2d470-151">Try this sample</span></span>

<span data-ttu-id="2d470-152">tootry forgatókönyv üzembe helyezését egy teljesen működőképes logic app és minta AS2, lásd: hello [AS2 logic app-sablon és a forgatókönyv](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span><span class="sxs-lookup"><span data-stu-id="2d470-152">tootry deploying a fully operational logic app and sample AS2 scenario, see hello [AS2 logic app template and scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="2d470-153">Nézet hello swagger</span><span class="sxs-lookup"><span data-stu-id="2d470-153">View hello swagger</span></span>
<span data-ttu-id="2d470-154">Lásd: hello [részletek swagger](/connectors/as2/).</span><span class="sxs-lookup"><span data-stu-id="2d470-154">See hello [swagger details](/connectors/as2/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2d470-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d470-155">Next steps</span></span>
[<span data-ttu-id="2d470-156">További tudnivalók hello vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="2d470-156">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md) 

