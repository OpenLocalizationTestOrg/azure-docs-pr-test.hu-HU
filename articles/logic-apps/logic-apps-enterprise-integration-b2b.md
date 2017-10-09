---
title: "aaaCreate B2B megoldások - Azure Logic Apps |} Microsoft Docs"
description: "Adatfogadás logic Apps alkalmazásokat a vállalati integrációs csomag hello hello B2B szolgáltatások segítségével"
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
ms.openlocfilehash: 8f01318a0415d81c37b216f9b991c060edec2053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="receive-data-in-logic-apps-with-hello-b2b-features-in-hello-enterprise-integration-pack"></a><span data-ttu-id="f73e6-103">Adatfogadás a logic apps funkcióival hello B2B hello vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="f73e6-103">Receive data in logic apps with hello B2B features in hello Enterprise Integration Pack</span></span>

<span data-ttu-id="f73e6-104">Miután létrehozta a partnerek és megállapodások integrációs fiókkal, készen áll a toocreate üzleti toobusiness (B2B) munkamenet-e a logikai alkalmazásnak a hello [vállalati integrációs csomag](logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f73e6-104">After you create an integration account that has partners and agreements, you are ready toocreate a business toobusiness (B2B) workflow for your logic app with hello [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f73e6-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f73e6-105">Prerequisites</span></span>

<span data-ttu-id="f73e6-106">toouse hello AS2 és X12 műveleteket, rendelkeznie kell egy vállalati integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="f73e6-106">toouse hello AS2 and X12 actions, you must have an Enterprise Integration Account.</span></span> <span data-ttu-id="f73e6-107">Ismerje meg, [hogyan toocreate vállalati integrációs fiók](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="f73e6-107">Learn [how toocreate an Enterprise Integration Account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

## <a name="create-a-logic-app-with-b2b-connectors"></a><span data-ttu-id="f73e6-108">Hozzon létre egy logic app B2B összekötők</span><span class="sxs-lookup"><span data-stu-id="f73e6-108">Create a logic app with B2B connectors</span></span>

<span data-ttu-id="f73e6-109">Kövesse az alábbi lépéseket toocreate a B2B logikai alkalmazás hello AS2 és X12 használó műveletek tooreceive adatait egy kereskedelmi partner:</span><span class="sxs-lookup"><span data-stu-id="f73e6-109">Follow these steps toocreate a B2B logic app that uses hello AS2 and X12 actions tooreceive data from a trading partner:</span></span>

1. <span data-ttu-id="f73e6-110">Hozzon létre egy logikai alkalmazást, majd [a app tooyour integrációs fiókját](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="f73e6-110">Create a logic app, then [link your app tooyour integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

2. <span data-ttu-id="f73e6-111">Adja hozzá a **kérelem - amikor egy HTTP-kérelem érkezik** eseményindító tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f73e6-111">Add a **Request - When an HTTP request is received** trigger tooyour logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. <span data-ttu-id="f73e6-112">tooadd hello **dekódolása AS2** művelet, jelölje be **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f73e6-112">tooadd hello **Decode AS2** action, select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. <span data-ttu-id="f73e6-113">toofilter összes műveletek toohello egy használni kívánt, adja meg a hello word **as2** hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f73e6-113">toofilter all actions toohello one that you want, enter hello word **as2** in hello search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. <span data-ttu-id="f73e6-114">Jelölje be hello **AS2 - dekódolási AS2 üzenet** művelet.</span><span class="sxs-lookup"><span data-stu-id="f73e6-114">Select hello **AS2 - Decode AS2 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. <span data-ttu-id="f73e6-115">Adja hozzá a hello **törzs** , amelyet az toouse bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="f73e6-115">Add hello **Body** that you want toouse as input.</span></span> <span data-ttu-id="f73e6-116">Ebben a példában válassza ki, hogy az eseményindítók hello logikai alkalmazás hello HTTP-kérelem hello törzsét.</span><span class="sxs-lookup"><span data-stu-id="f73e6-116">In this example, select hello body of hello HTTP request that triggers hello logic app.</span></span> <span data-ttu-id="f73e6-117">Adjon meg egy kifejezést, amely a bemeneti hello fejlécének hello vagy **FEJLÉCEK** mező:</span><span class="sxs-lookup"><span data-stu-id="f73e6-117">Or enter an expression that inputs hello headers in hello **HEADERS** field:</span></span>

    <span data-ttu-id="f73e6-118">@triggerOutputs(["fejléc"])</span><span class="sxs-lookup"><span data-stu-id="f73e6-118">@triggerOutputs()['headers']</span></span>

7. <span data-ttu-id="f73e6-119">Adja hozzá a szükséges hello **fejlécek** az AS2, amely hello HTTP-kérelemfejlécekben található.</span><span class="sxs-lookup"><span data-stu-id="f73e6-119">Add hello required **Headers** for AS2, which you can find in hello HTTP request headers.</span></span> <span data-ttu-id="f73e6-120">Ebben a példában válassza hello fejléceket a HTTP-kérelem hello eseményindító hello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f73e6-120">In this example, select hello headers of hello HTTP request that trigger hello logic app.</span></span>

8. <span data-ttu-id="f73e6-121">Mostantól hozzáadhatja azok hello dekódolási X12 üzenet művelet.</span><span class="sxs-lookup"><span data-stu-id="f73e6-121">Now add hello Decode X12 message action.</span></span> <span data-ttu-id="f73e6-122">Válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f73e6-122">Select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. <span data-ttu-id="f73e6-123">toofilter összes műveletek toohello egy használni kívánt, adja meg a hello word **x12** hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f73e6-123">toofilter all actions toohello one that you want, enter hello word **x12** in hello search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. <span data-ttu-id="f73e6-124">Jelölje be hello **X12-dekódolási X12 üzenet** művelet.</span><span class="sxs-lookup"><span data-stu-id="f73e6-124">Select hello **X12 - Decode X12 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. <span data-ttu-id="f73e6-125">Most meg kell adnia hello bemeneti toothis műveletet.</span><span class="sxs-lookup"><span data-stu-id="f73e6-125">Now you must specify hello input toothis action.</span></span> <span data-ttu-id="f73e6-126">A bemeneti érték hello előző AS2 művelet hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="f73e6-126">This input is hello output from hello previous AS2 action.</span></span>

    <span data-ttu-id="f73e6-127">hello tényleges üzenet tartalma egy JSON-objektum és base64-kódolású, ezért meg kell adnia egy kifejezés hello bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="f73e6-127">hello actual message content is in a JSON object and is base64 encoded, so you must specify an expression as hello input.</span></span> 
    <span data-ttu-id="f73e6-128">Adja meg a következő kifejezés hello hello **X12 lapos fájl üzenet tooDECODE** beviteli mezőt:</span><span class="sxs-lookup"><span data-stu-id="f73e6-128">Enter hello following expression in hello **X12 FLAT FILE MESSAGE tooDECODE** input field:</span></span>
    
    <span data-ttu-id="f73e6-129">@base64ToString(body('Decode_AS2_message')? ["AS2Message']? ["Tartalom"])</span><span class="sxs-lookup"><span data-stu-id="f73e6-129">@base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])</span></span>

    <span data-ttu-id="f73e6-130">Most vegyen fel olyan lépéseket toodecode hello X12 adatok kereskedelmi partner hello kapott, és a kimeneti JSON-objektum elemek.</span><span class="sxs-lookup"><span data-stu-id="f73e6-130">Now add steps toodecode hello X12 data received from hello trading partner and output items in a JSON object.</span></span> 
    <span data-ttu-id="f73e6-131">adatok hello toonotify hello partner érkezett, küldhet vissza a válaszban hello AS2 üzenet törlése értesítés (MDN) a HTTP-válasz művelethez.</span><span class="sxs-lookup"><span data-stu-id="f73e6-131">toonotify hello partner that hello data was received, you can send back a response containing hello AS2 Message Disposition Notification (MDN) in an HTTP Response Action.</span></span>

12. <span data-ttu-id="f73e6-132">tooadd hello **válasz** művelet, válassza a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f73e6-132">tooadd hello **Response** action, choose **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. <span data-ttu-id="f73e6-133">toofilter összes műveletek toohello egy használni kívánt, adja meg a hello word **válasz** hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f73e6-133">toofilter all actions toohello one that you want, enter hello word **response** in hello search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. <span data-ttu-id="f73e6-134">Jelölje be hello **válasz** művelet.</span><span class="sxs-lookup"><span data-stu-id="f73e6-134">Select hello **Response** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. <span data-ttu-id="f73e6-135">tooaccess MDN hello hello hello kimenetét a **dekódolási X12 üzenet** művelet, a set hello válasz **törzs** mezőt a ebben a kifejezésben:</span><span class="sxs-lookup"><span data-stu-id="f73e6-135">tooaccess hello MDN from hello output of hello **Decode X12 message** action, set hello response **BODY** field with this expression:</span></span>

    <span data-ttu-id="f73e6-136">@base64ToString(body('Decode_AS2_message')? ["OutgoingMdn']? ["Tartalom"])</span><span class="sxs-lookup"><span data-stu-id="f73e6-136">@base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. <span data-ttu-id="f73e6-137">Mentse a munkáját.</span><span class="sxs-lookup"><span data-stu-id="f73e6-137">Save your work.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

<span data-ttu-id="f73e6-138">Most már befejezte a B2B logikai alkalmazás beállítása.</span><span class="sxs-lookup"><span data-stu-id="f73e6-138">You are now done setting up your B2B logic app.</span></span> <span data-ttu-id="f73e6-139">Egy valós alkalmazás érdemes toostore hello dekódolni X12 adatok-üzletági (LOB) alkalmazás vagy adatok tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f73e6-139">In a real world application, you might want toostore hello decoded X12 data in a line-of-business (LOB) app or data store.</span></span> <span data-ttu-id="f73e6-140">tooconnect a saját ÜZLETÁGI alkalmazásai és ezen API-k használata a Logic Apps alkalmazást, vagy adhat hozzá további műveletek írása egyéni API-k.</span><span class="sxs-lookup"><span data-stu-id="f73e6-140">tooconnect your own LOB apps and use these APIs in your logic app, you can add further actions or write custom APIs.</span></span>

## <a name="features-and-use-cases"></a><span data-ttu-id="f73e6-141">Szolgáltatások és a használati esetek</span><span class="sxs-lookup"><span data-stu-id="f73e6-141">Features and use cases</span></span>

* <span data-ttu-id="f73e6-142">hello AS2 és X12 dekódolni, és műveletek kódolása lehetővé, hogy adatcserében működik közre a logic apps iparági szabványos protokollok segítségével kereskedelmi partnerek között.</span><span class="sxs-lookup"><span data-stu-id="f73e6-142">hello AS2 and X12 decode and encode actions let you exchange data between trading partners by using industry standard protocols in logic apps.</span></span>
* <span data-ttu-id="f73e6-143">tooexchange adatok az üzleti partnerekkel való, használható AS2 és X12 vagy anélkül egymással.</span><span class="sxs-lookup"><span data-stu-id="f73e6-143">tooexchange data with trading partners, you can use AS2 and X12 with or without each other.</span></span>
* <span data-ttu-id="f73e6-144">hello B2B műveletek segítséget nyújt a partnerek és a megállapodások az integráció fiókban könnyen létrehozását és felhasználását azokat a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f73e6-144">hello B2B actions help you create partners and agreements easily in your integration account and consume them in a logic app.</span></span>
* <span data-ttu-id="f73e6-145">A Logic Apps alkalmazást az egyéb műveletek bővítésekor küldhet és fogadhat adatokat más alkalmazásokat és szolgáltatásokat, mint a SalesForce között.</span><span class="sxs-lookup"><span data-stu-id="f73e6-145">When you extend your logic app with other actions, you can send and receive data between other apps and services like SalesForce.</span></span>

## <a name="learn-more"></a><span data-ttu-id="f73e6-146">Részletek</span><span class="sxs-lookup"><span data-stu-id="f73e6-146">Learn more</span></span>
[<span data-ttu-id="f73e6-147">További tudnivalók hello vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="f73e6-147">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md)
