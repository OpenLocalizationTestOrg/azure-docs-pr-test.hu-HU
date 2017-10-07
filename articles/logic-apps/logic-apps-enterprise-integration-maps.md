---
title: "az XSLT-leképezések - Azure Logic Apps XML aaaTransform |} Microsoft Docs"
description: "XSLT-leképezések tootransform XML-adatok az Azure Logic Apps és hello vállalati integrációs csomag hozzáadása"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 720e6f988b8542136dfcc402c3c463fcfb2f23cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-maps-for-xml-data-transform"></a><span data-ttu-id="3c094-103">Az XML-adatok átalakító maps hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3c094-103">Add maps for XML data transform</span></span>

<span data-ttu-id="3c094-104">Vállalati integrációs használ a maps tootransform XML-adatok között.</span><span class="sxs-lookup"><span data-stu-id="3c094-104">Enterprise integration uses maps tootransform XML data between formats.</span></span> <span data-ttu-id="3c094-105">A térkép az XML-dokumentum, egy dokumentumot, amely egy másik formátumra alakítja át kell hello adatok definiáló.</span><span class="sxs-lookup"><span data-stu-id="3c094-105">A map is an XML document that defines hello data in a document that should be transformed into another format.</span></span> 

## <a name="why-use-maps"></a><span data-ttu-id="3c094-106">A maps miért érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="3c094-106">Why use maps?</span></span>

<span data-ttu-id="3c094-107">Tegyük fel, hogy rendszeresen kapott B2B rendeléseket és a számlák hello YYYMMDD formátumot használja a dátumok ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="3c094-107">Suppose that you regularly receive B2B orders or invoices from a customer who uses hello YYYMMDD format for dates.</span></span> <span data-ttu-id="3c094-108">Azonban a szervezet tárolási hello MMDDYYY formátumú dátum.</span><span class="sxs-lookup"><span data-stu-id="3c094-108">However, in your organization, you store dates in hello MMDDYYY format.</span></span> <span data-ttu-id="3c094-109">Használhatja a térkép túl*átalakítási* hello YYYMMDD dátumformátum hello MMDDYYY előtt hello sorrend, illetve a Számla részletek tárolni a felhasználói tevékenység adatbázisban történő.</span><span class="sxs-lookup"><span data-stu-id="3c094-109">You can use a map too*transform* hello YYYMMDD date format into hello MMDDYYY before storing hello order or invoice details in your customer activity database.</span></span>

## <a name="how-do-i-create-a-map"></a><span data-ttu-id="3c094-110">Hogyan térkép létrehozásához?</span><span class="sxs-lookup"><span data-stu-id="3c094-110">How do I create a map?</span></span>

<span data-ttu-id="3c094-111">BizTalk integrációs projekteket hozhat létre hello [vállalati integrációs csomag](logic-apps-enterprise-integration-overview.md "további információ a hello vállalati integrációs csomag") a Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="3c094-111">You can create BizTalk Integration projects with hello [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about hello enterprise integration pack") for Visual Studio 2015.</span></span> <span data-ttu-id="3c094-112">Ezután létrehozhat olyan integrációs térképe fájlt, amely lehetővé teszi az elemek között két XML-sémafájlok vizuálisan hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="3c094-112">You can then create an Integration Map file that lets you visually map items between two XML schema files.</span></span> <span data-ttu-id="3c094-113">Ez a projekt létrehozása után fog az XSLT-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="3c094-113">After you build this project, you will have an XSLT document.</span></span>

## <a name="how-do-i-add-a-map"></a><span data-ttu-id="3c094-114">Hogyan vegyen fel egy társítást?</span><span class="sxs-lookup"><span data-stu-id="3c094-114">How do I add a map?</span></span>

1. <span data-ttu-id="3c094-115">Hello Azure-portálon, válassza ki **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="3c094-115">In hello Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="3c094-116">Hello szűrő keresési mezőbe, írja be a **integrációs**, majd jelölje be **integrációs fiókok** hello eredmények listából.</span><span class="sxs-lookup"><span data-stu-id="3c094-116">In hello filter search box, enter **integration**, then select **Integration Accounts** from hello results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="3c094-117">Válassza ki a hello integrációs fiókra, amelyhez tooadd hello leképezés.</span><span class="sxs-lookup"><span data-stu-id="3c094-117">Select hello integration account where you want tooadd hello map.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="3c094-118">Jelölje be hello **Maps** csempére.</span><span class="sxs-lookup"><span data-stu-id="3c094-118">Select hello **Maps** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. <span data-ttu-id="3c094-119">Miután hello Maps panel nyílik meg, válassza ki azt **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3c094-119">After hello Maps blade opens, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. <span data-ttu-id="3c094-120">Adjon meg egy **neve** a térképen.</span><span class="sxs-lookup"><span data-stu-id="3c094-120">Enter a **Name** for your map.</span></span> <span data-ttu-id="3c094-121">tooupload hello térkép fájlt, válassza ki a hello ikonja hello hello jobb oldalán **térkép** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="3c094-121">tooupload hello map file, choose hello folder icon on hello right side of hello **Map** text box.</span></span> <span data-ttu-id="3c094-122">Hello feltöltési folyamat befejezése után válassza ki a **OK**.</span><span class="sxs-lookup"><span data-stu-id="3c094-122">After hello upload process completes, choose **OK**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. <span data-ttu-id="3c094-123">Miután az Azure ad hello térkép tooyour integrációs fiók, bemutatja, hogy a leképezés vagy nem lett-e adva képernyőn üzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="3c094-123">After Azure adds hello map tooyour integration account, you get an onscreen message that shows whether your map file was added or not.</span></span> <span data-ttu-id="3c094-124">Miután ezt az üzenetet kap, válassza ki azt a hello **Maps** csempe szeretné megjeleníteni az hello újonnan hozzáadott leképezés.</span><span class="sxs-lookup"><span data-stu-id="3c094-124">After you get this message, choose hello **Maps** tile so you can view hello newly added map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a><span data-ttu-id="3c094-125">Hogyan szerkeszthetők a térkép?</span><span class="sxs-lookup"><span data-stu-id="3c094-125">How do I edit a map?</span></span>

<span data-ttu-id="3c094-126">Új leképezési fájl hello módosítások kell feltöltenie.</span><span class="sxs-lookup"><span data-stu-id="3c094-126">You must upload a new map file with hello changes that you want.</span></span> <span data-ttu-id="3c094-127">Először letöltheti hello térkép szerkesztésre.</span><span class="sxs-lookup"><span data-stu-id="3c094-127">You can first download hello map for editing.</span></span>

<span data-ttu-id="3c094-128">tooupload új leképezés, amely kiváltja a hello meglévő térkép, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="3c094-128">tooupload a new map that replaces hello existing map, follow these steps.</span></span>

1. <span data-ttu-id="3c094-129">Válassza ki a hello **Maps** csempére.</span><span class="sxs-lookup"><span data-stu-id="3c094-129">Choose hello **Maps** tile.</span></span>

2. <span data-ttu-id="3c094-130">Után hello Maps panel nyílik meg, válassza ki a megjeleníteni kívánt tooedit hello leképezés.</span><span class="sxs-lookup"><span data-stu-id="3c094-130">After hello Maps blade opens, select hello map that you want tooedit.</span></span>

3. <span data-ttu-id="3c094-131">A hello **Maps** paneljén válassza **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="3c094-131">On hello **Maps** blade, choose **Update**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. <span data-ttu-id="3c094-132">A hello fájlválasztó, válassza ki a hello nyilvántartását fájlját, hogy szeretné, hogy tooupload, majd válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="3c094-132">In hello file picker, select hello map file that you want tooupload, then select **Open**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-toodelete-a-map"></a><span data-ttu-id="3c094-133">Hogyan toodelete térképre?</span><span class="sxs-lookup"><span data-stu-id="3c094-133">How toodelete a map?</span></span>

1. <span data-ttu-id="3c094-134">Válassza ki a hello **Maps** csempére.</span><span class="sxs-lookup"><span data-stu-id="3c094-134">Choose hello **Maps** tile.</span></span>

2. <span data-ttu-id="3c094-135">Után hello Maps panel nyílik meg, válassza ki a kívánt toodelete hello leképezés.</span><span class="sxs-lookup"><span data-stu-id="3c094-135">After hello Maps blade opens, select hello map you want toodelete.</span></span>

3. <span data-ttu-id="3c094-136">Válasszon **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3c094-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. <span data-ttu-id="3c094-137">Győződjön meg arról, hogy szeretné-e toodelete hello leképezés.</span><span class="sxs-lookup"><span data-stu-id="3c094-137">Confirm that you want toodelete hello map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a><span data-ttu-id="3c094-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3c094-138">Next Steps</span></span>
* [<span data-ttu-id="3c094-139">További tudnivalók a vállalati integrációs csomag hello</span><span class="sxs-lookup"><span data-stu-id="3c094-139">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")  
* [<span data-ttu-id="3c094-140">További információ a megállapodások</span><span class="sxs-lookup"><span data-stu-id="3c094-140">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  
* [<span data-ttu-id="3c094-141">További tudnivalók átalakítások</span><span class="sxs-lookup"><span data-stu-id="3c094-141">Learn more about transforms</span></span>](logic-apps-enterprise-integration-transform.md "vállalati integrációs átalakítások ismertetése")  

