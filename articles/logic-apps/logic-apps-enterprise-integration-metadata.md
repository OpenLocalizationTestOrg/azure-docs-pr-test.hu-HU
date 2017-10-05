---
title: "Integráció fiók összetevő metaadat - Azure Logic Apps alkalmazásokat kezeléséhez |} Microsoft Docs"
description: "Adja hozzá vagy integrációs fiókok az Azure Logic Apps összetevő metaadatok lekérése"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 11/21/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 28bb8296ddd820ec5aa9793dc0928b4b1e67bf6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a><span data-ttu-id="51121-103">Integrációs fiókok logic Apps alkalmazásokat a hitelesítendő metaadatok kezelése</span><span class="sxs-lookup"><span data-stu-id="51121-103">Manage artifact metadata in integration accounts for logic apps</span></span>

<span data-ttu-id="51121-104">Integrációs fiókok egyéni metaadatait az összetevők ad meg, és a metaadatok lekérése futtatás során a logikai alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="51121-104">You can define custom metadata for artifacts in integration accounts and retrieve that metadata during runtime for your logic app.</span></span> <span data-ttu-id="51121-105">Például megadhatja, hogy az összetevők, például a partnerek, egyezmények, sémák és maps metaadat - összes metaadatok kulcs-érték párok tárolására.</span><span class="sxs-lookup"><span data-stu-id="51121-105">For example, you can specify metadata for artifacts like partners, agreements, schemas, and maps - all store metadata using key-value pairs.</span></span> <span data-ttu-id="51121-106">Jelenleg az összetevők nem hozható létre a metaadatok felhasználói felületén keresztül, de a REST API-k hozhat létre a metaadatok.</span><span class="sxs-lookup"><span data-stu-id="51121-106">Currently, artifacts can't create metadata through UI, but you can use REST APIs to create metadata.</span></span> <span data-ttu-id="51121-107">Metaadatok hozzáadása a létrehozásakor vagy egy partner, a szerződés vagy a séma kiválasztása az Azure portálon, válassza a **módosítsa a JSON**.</span><span class="sxs-lookup"><span data-stu-id="51121-107">To add metadata when you create or select a partner, agreement, or schema in the Azure portal, choose **Edit as JSON**.</span></span> <span data-ttu-id="51121-108">Logic Apps alkalmazásokat a hitelesítendő metaadatainak beolvasására, használhatja a összetevő Fiókkeresés integrációs szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="51121-108">To retrieve artifact metadata in logic apps, you can use the Integration Account Artifact Lookup feature.</span></span>

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a><span data-ttu-id="51121-109">Integrációs fiókok lévő metaadatok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="51121-109">Add metadata to artifacts in integration accounts</span></span>

1. <span data-ttu-id="51121-110">Hozzon létre egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md).</span><span class="sxs-lookup"><span data-stu-id="51121-110">Create an [integration account](logic-apps-enterprise-integration-create-integration-account.md).</span></span>

2. <span data-ttu-id="51121-111">Adja hozzá az összetevő integrációs fiókjába, egy [partner](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [megállapodás](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), vagy [séma](logic-apps-enterprise-integration-schemas.md).</span><span class="sxs-lookup"><span data-stu-id="51121-111">Add an artifact to your integration account, for example, a [partner](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [agreement](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), or [schema](logic-apps-enterprise-integration-schemas.md).</span></span>

3.  <span data-ttu-id="51121-112">Válassza ki a összetevő, válassza a **módosítsa a JSON**, és írja be a metaadatok adatait.</span><span class="sxs-lookup"><span data-stu-id="51121-112">Select the artifact, choose **Edit as JSON**, and enter metadata details.</span></span>

    ![Adja meg a metaadatok](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a><span data-ttu-id="51121-114">A logic apps összetevőkhöz metaadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="51121-114">Retrieve metadata from artifacts for logic apps</span></span>

1. <span data-ttu-id="51121-115">Hozzon létre egy [logikai alkalmazás](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="51121-115">Create a [logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="51121-116">Hozzon létre egy [integrációs fiókjába a Logic Apps alkalmazást kapcsolatot](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app).</span><span class="sxs-lookup"><span data-stu-id="51121-116">Create a [link from your logic app to your integration account](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app).</span></span> 

3. <span data-ttu-id="51121-117">Logic App Designer, adja hozzá egy eseményindító például *kérelem* vagy *HTTP* a logikai alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="51121-117">In Logic App Designer, add a trigger like *Request* or *HTTP* to your logic app.</span></span>

4.  <span data-ttu-id="51121-118">Válasszon **tovább** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="51121-118">Choose **Next Step** > **Add an action**.</span></span> <span data-ttu-id="51121-119">Keresse meg *integrációs* , keresse meg és jelölje **integrációs fiók - integráció összetevő Fiókkeresés**.</span><span class="sxs-lookup"><span data-stu-id="51121-119">Search for *integration* so you can find and then select **Integration Account - Integration Account Artifact Lookup**.</span></span>

    ![Válassza ki az integráció összetevő Fiókkeresés](media/logic-apps-enterprise-integration-metadata/image2.png)

5. <span data-ttu-id="51121-121">Válassza ki a **összetevő típus**, és adja meg a **Adatösszetevőt nevét**.</span><span class="sxs-lookup"><span data-stu-id="51121-121">Select the **Artifact Type**, and provide the **Artifact Name**.</span></span>

    ![Összetevő típusa, és adja meg az összetevő neve](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a><span data-ttu-id="51121-123">Példa: Beolvasni az erőforráspartner adatokat</span><span class="sxs-lookup"><span data-stu-id="51121-123">Example: Retrieve partner metadata</span></span>

<span data-ttu-id="51121-124">Partner metaadatok rendelkezik, ezek `routingUrl` részletek:</span><span class="sxs-lookup"><span data-stu-id="51121-124">Partner metadata has these `routingUrl` details:</span></span>

![Partner "routingURL" metaadatok keresése](media/logic-apps-enterprise-integration-metadata/image6.png)

1. <span data-ttu-id="51121-126">A Logic Apps alkalmazást, adja hozzá az eseményindító egy **integrációs fiók - integráció összetevő Fiókkeresés** művelet esetén a partner és egy **HTTP**.</span><span class="sxs-lookup"><span data-stu-id="51121-126">In your logic app, add your trigger, an **Integration Account - Integration Account Artifact Lookup** action for your partner, and an **HTTP**.</span></span>

    ![A Logic Apps alkalmazást eseményindító, a keresési összetevő és a "HTTP" hozzáadása](media/logic-apps-enterprise-integration-metadata/image4.png)

2. <span data-ttu-id="51121-128">Az URI azonosító lekéréséhez használja a logikai alkalmazásnak kód nézetre.</span><span class="sxs-lookup"><span data-stu-id="51121-128">To retrieve the URI, go to Code View for your logic app.</span></span> <span data-ttu-id="51121-129">A logic app-definíciót a példához hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="51121-129">Your logic app definition should look like this example:</span></span>

    ![Keresési keresési](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a><span data-ttu-id="51121-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51121-131">Next steps</span></span>
* [<span data-ttu-id="51121-132">További információ a megállapodások</span><span class="sxs-lookup"><span data-stu-id="51121-132">Learn more about agreements</span></span>](logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  
