---
title: "Hozzon létre B2B megoldások - Azure Logic Apps |} Microsoft Docs"
description: "Adatfogadás logic Apps alkalmazásokat a vállalati integrációs csomag a B2B szolgáltatások segítségével"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 0625787ddcbc0091e70b111f687e25929720ad15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="receive-data-in-logic-apps-with-the-b2b-features-in-the-enterprise-integration-pack"></a><span data-ttu-id="959ba-103">Adatfogadás a logic apps B2B funkcióit a vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="959ba-103">Receive data in logic apps with the B2B features in the Enterprise Integration Pack</span></span>

<span data-ttu-id="959ba-104">Miután létrehozta a partnerek és megállapodások integrációs fiókkal, készen áll a Logic Apps alkalmazást, a vállalatok számára (B2B) munkafolyamat létrehozása a [vállalati integrációs csomag](logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="959ba-104">After you create an integration account that has partners and agreements, you are ready to create a business to business (B2B) workflow for your logic app with the [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="959ba-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="959ba-105">Prerequisites</span></span>

<span data-ttu-id="959ba-106">Az AS2 és X12 műveleteket, rendelkeznie kell egy vállalati integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="959ba-106">To use the AS2 and X12 actions, you must have an Enterprise Integration Account.</span></span> <span data-ttu-id="959ba-107">Ismerje meg, [vállalati integrációs fiók létrehozása](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="959ba-107">Learn [how to create an Enterprise Integration Account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

## <a name="create-a-logic-app-with-b2b-connectors"></a><span data-ttu-id="959ba-108">Hozzon létre egy logic app B2B összekötők</span><span class="sxs-lookup"><span data-stu-id="959ba-108">Create a logic app with B2B connectors</span></span>

<span data-ttu-id="959ba-109">Kövesse az alábbi lépéseket az AS2 és X12 használó B2B logikai alkalmazás létrehozása az adatok fogadására a kereskedelmi partnertől műveletek:</span><span class="sxs-lookup"><span data-stu-id="959ba-109">Follow these steps to create a B2B logic app that uses the AS2 and X12 actions to receive data from a trading partner:</span></span>

1. <span data-ttu-id="959ba-110">Hozzon létre egy logikai alkalmazást, majd [az alkalmazás a integrációs fiókkal összekapcsolni](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="959ba-110">Create a logic app, then [link your app to your integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

2. <span data-ttu-id="959ba-111">Adja hozzá a **kérelem - amikor egy HTTP-kérelem érkezik** eseményindító a logikai alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="959ba-111">Add a **Request - When an HTTP request is received** trigger to your logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. <span data-ttu-id="959ba-112">Hozzáadása a **dekódolása AS2** művelet, jelölje be **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="959ba-112">To add the **Decode AS2** action, select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. <span data-ttu-id="959ba-113">Ha szeretne szűrni a használni kívánt összes műveleteket, adja meg a word **as2** be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="959ba-113">To filter all actions to the one that you want, enter the word **as2** in the search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. <span data-ttu-id="959ba-114">Válassza ki a **AS2 - dekódolási AS2 üzenet** művelet.</span><span class="sxs-lookup"><span data-stu-id="959ba-114">Select the **AS2 - Decode AS2 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. <span data-ttu-id="959ba-115">Adja hozzá a **törzs** bemenetként használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="959ba-115">Add the **Body** that you want to use as input.</span></span> <span data-ttu-id="959ba-116">Ebben a példában válassza ki, amely elindítja a logikai alkalmazás a HTTP-kérelem törzsét.</span><span class="sxs-lookup"><span data-stu-id="959ba-116">In this example, select the body of the HTTP request that triggers the logic app.</span></span> <span data-ttu-id="959ba-117">Adjon meg egy kifejezést, amely a bemeneti fejlécének vagy a **FEJLÉCEK** mező:</span><span class="sxs-lookup"><span data-stu-id="959ba-117">Or enter an expression that inputs the headers in the **HEADERS** field:</span></span>

    <span data-ttu-id="959ba-118">@triggerOutputs(["fejléc"])</span><span class="sxs-lookup"><span data-stu-id="959ba-118">@triggerOutputs()['headers']</span></span>

7. <span data-ttu-id="959ba-119">Adja hozzá a szükséges **fejlécek** az AS2, amelyek a HTTP-kérelmek fejléceinek található.</span><span class="sxs-lookup"><span data-stu-id="959ba-119">Add the required **Headers** for AS2, which you can find in the HTTP request headers.</span></span> <span data-ttu-id="959ba-120">Ebben a példában válassza ki, amelynek hatására a logikai alkalmazást a HTTP-kérés fejlécébe.</span><span class="sxs-lookup"><span data-stu-id="959ba-120">In this example, select the headers of the HTTP request that trigger the logic app.</span></span>

8. <span data-ttu-id="959ba-121">Most adja hozzá a dekódolási X12 üzenet művelet.</span><span class="sxs-lookup"><span data-stu-id="959ba-121">Now add the Decode X12 message action.</span></span> <span data-ttu-id="959ba-122">Válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="959ba-122">Select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. <span data-ttu-id="959ba-123">Ha szeretne szűrni a használni kívánt összes műveleteket, adja meg a word **x12** be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="959ba-123">To filter all actions to the one that you want, enter the word **x12** in the search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. <span data-ttu-id="959ba-124">Válassza ki a **X12-dekódolási X12 üzenet** művelet.</span><span class="sxs-lookup"><span data-stu-id="959ba-124">Select the **X12 - Decode X12 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. <span data-ttu-id="959ba-125">Ez a művelet bemeneti most meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="959ba-125">Now you must specify the input to this action.</span></span> <span data-ttu-id="959ba-126">A bemeneti érték az előző AS2 művelet eredményének.</span><span class="sxs-lookup"><span data-stu-id="959ba-126">This input is the output from the previous AS2 action.</span></span>

    <span data-ttu-id="959ba-127">A tényleges üzenet tartalma egy JSON-objektum és base64-kódolású, ezért meg kell adnia egy kifejezést a bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="959ba-127">The actual message content is in a JSON object and is base64 encoded, so you must specify an expression as the input.</span></span> 
    <span data-ttu-id="959ba-128">Adja meg a következő kifejezésre a **X12 lapos fájl üzenet TO DEKÓDOLÁSI** beviteli mezőt:</span><span class="sxs-lookup"><span data-stu-id="959ba-128">Enter the following expression in the **X12 FLAT FILE MESSAGE TO DECODE** input field:</span></span>
    
    <span data-ttu-id="959ba-129">@base64ToString(body('Decode_AS2_message')? ["AS2Message']? ["Tartalom"])</span><span class="sxs-lookup"><span data-stu-id="959ba-129">@base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])</span></span>

    <span data-ttu-id="959ba-130">Most vegyen fel olyan lépéseket a X12 adatok kereskedelmi partneren, és egy JSON-objektum elemeinek kimeneti dekódolására.</span><span class="sxs-lookup"><span data-stu-id="959ba-130">Now add steps to decode the X12 data received from the trading partner and output items in a JSON object.</span></span> 
    <span data-ttu-id="959ba-131">Az értesítés a partner, hogy az adatokat fogadta a program, küldhet vissza egy válaszban az AS2 üzenet törlése értesítés (MDN) a HTTP-válasz művelethez.</span><span class="sxs-lookup"><span data-stu-id="959ba-131">To notify the partner that the data was received, you can send back a response containing the AS2 Message Disposition Notification (MDN) in an HTTP Response Action.</span></span>

12. <span data-ttu-id="959ba-132">Hozzáadása a **válasz** művelet, válassza a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="959ba-132">To add the **Response** action, choose **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. <span data-ttu-id="959ba-133">Ha szeretne szűrni a használni kívánt összes műveleteket, adja meg a word **válasz** be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="959ba-133">To filter all actions to the one that you want, enter the word **response** in the search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. <span data-ttu-id="959ba-134">Válassza ki a **válasz** művelet.</span><span class="sxs-lookup"><span data-stu-id="959ba-134">Select the **Response** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. <span data-ttu-id="959ba-135">A MDN elérje a kimenetét a **dekódolási X12 üzenet** művelet, állítsa be a válasz **törzs** mezőt a ebben a kifejezésben:</span><span class="sxs-lookup"><span data-stu-id="959ba-135">To access the MDN from the output of the **Decode X12 message** action, set the response **BODY** field with this expression:</span></span>

    <span data-ttu-id="959ba-136">@base64ToString(body('Decode_AS2_message')? ["OutgoingMdn']? ["Tartalom"])</span><span class="sxs-lookup"><span data-stu-id="959ba-136">@base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. <span data-ttu-id="959ba-137">Mentse a munkáját.</span><span class="sxs-lookup"><span data-stu-id="959ba-137">Save your work.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

<span data-ttu-id="959ba-138">Most már befejezte a B2B logikai alkalmazás beállítása.</span><span class="sxs-lookup"><span data-stu-id="959ba-138">You are now done setting up your B2B logic app.</span></span> <span data-ttu-id="959ba-139">Egy valós alkalmazás, előfordulhat, hogy tárolni szeretné a dekódolt X12 adatok-üzletági (LOB) alkalmazás vagy adatok tárolóban.</span><span class="sxs-lookup"><span data-stu-id="959ba-139">In a real world application, you might want to store the decoded X12 data in a line-of-business (LOB) app or data store.</span></span> <span data-ttu-id="959ba-140">Kapcsolódás saját LOB-alkalmazások, és ezen API-k használata a Logic Apps alkalmazást, adhat hozzá további műveletek vagy egyéni API-k írása.</span><span class="sxs-lookup"><span data-stu-id="959ba-140">To connect your own LOB apps and use these APIs in your logic app, you can add further actions or write custom APIs.</span></span>

## <a name="features-and-use-cases"></a><span data-ttu-id="959ba-141">Szolgáltatások és a használati esetek</span><span class="sxs-lookup"><span data-stu-id="959ba-141">Features and use cases</span></span>

* <span data-ttu-id="959ba-142">Az AS2 és X12 dekódolni, és műveletek kódolása lehetővé, hogy adatcserében működik közre a logic apps iparági szabványos protokollok segítségével kereskedelmi partnerek között.</span><span class="sxs-lookup"><span data-stu-id="959ba-142">The AS2 and X12 decode and encode actions let you exchange data between trading partners by using industry standard protocols in logic apps.</span></span>
* <span data-ttu-id="959ba-143">Az exchange-adatok az üzleti partnerekkel való, használhatja AS2 és X12 vagy anélkül egymással.</span><span class="sxs-lookup"><span data-stu-id="959ba-143">To exchange data with trading partners, you can use AS2 and X12 with or without each other.</span></span>
* <span data-ttu-id="959ba-144">A B2B műveletek hogyan hozhat létre partnerek és megállapodások könnyen integrációs fiókjában, és felhasználni azokat a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="959ba-144">The B2B actions help you create partners and agreements easily in your integration account and consume them in a logic app.</span></span>
* <span data-ttu-id="959ba-145">A Logic Apps alkalmazást az egyéb műveletek bővítésekor küldhet és fogadhat adatokat más alkalmazásokat és szolgáltatásokat, mint a SalesForce között.</span><span class="sxs-lookup"><span data-stu-id="959ba-145">When you extend your logic app with other actions, you can send and receive data between other apps and services like SalesForce.</span></span>

## <a name="learn-more"></a><span data-ttu-id="959ba-146">Részletek</span><span class="sxs-lookup"><span data-stu-id="959ba-146">Learn more</span></span>
[<span data-ttu-id="959ba-147">További tudnivalók a vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="959ba-147">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md)
