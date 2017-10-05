---
title: "Átalakítás XML XSLT a maps - Azure Logic Apps |} Microsoft Docs"
description: "XSLT van leképezve az Azure Logic Apps és a vállalati integrációs csomag XML-adatok hozzáadása"
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
ms.openlocfilehash: 4445a84a6c6425110e7d705019a28b5cc5447046
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-maps-for-xml-data-transform"></a><span data-ttu-id="e3145-103">Az XML-adatok átalakító maps hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e3145-103">Add maps for XML data transform</span></span>

<span data-ttu-id="e3145-104">Vállalati integrációs maps formátumok közötti XML-adatok átalakítására használja.</span><span class="sxs-lookup"><span data-stu-id="e3145-104">Enterprise integration uses maps to transform XML data between formats.</span></span> <span data-ttu-id="e3145-105">A térkép az XML-dokumentum, amely definiálja az adatokat a dokumentumban, amely egy másik formátumra alakítja át kell.</span><span class="sxs-lookup"><span data-stu-id="e3145-105">A map is an XML document that defines the data in a document that should be transformed into another format.</span></span> 

## <a name="why-use-maps"></a><span data-ttu-id="e3145-106">A maps miért érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="e3145-106">Why use maps?</span></span>

<span data-ttu-id="e3145-107">Tegyük fel, hogy rendszeresen kapott B2B rendeléseket és a számlák YYYMMDD formátumot használja, a dátumok ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="e3145-107">Suppose that you regularly receive B2B orders or invoices from a customer who uses the YYYMMDD format for dates.</span></span> <span data-ttu-id="e3145-108">A szervezet tárolhatja, hogy a MMDDYYY formátumú dátum.</span><span class="sxs-lookup"><span data-stu-id="e3145-108">However, in your organization, you store dates in the MMDDYYY format.</span></span> <span data-ttu-id="e3145-109">Olyan térképet használható *átalakítási* a YYYMMDD dátumformátumra előtt a sorrend, illetve a Számla részletek tárolni a felhasználói tevékenység adatbázisban MMDDYYY be.</span><span class="sxs-lookup"><span data-stu-id="e3145-109">You can use a map to *transform* the YYYMMDD date format into the MMDDYYY before storing the order or invoice details in your customer activity database.</span></span>

## <a name="how-do-i-create-a-map"></a><span data-ttu-id="e3145-110">Hogyan térkép létrehozásához?</span><span class="sxs-lookup"><span data-stu-id="e3145-110">How do I create a map?</span></span>

<span data-ttu-id="e3145-111">BizTalk integrációs-projektek hozhat létre a [vállalati integrációs csomag](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") a Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="e3145-111">You can create BizTalk Integration projects with the [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about the enterprise integration pack") for Visual Studio 2015.</span></span> <span data-ttu-id="e3145-112">Ezután létrehozhat olyan integrációs térképe fájlt, amely lehetővé teszi az elemek között két XML-sémafájlok vizuálisan hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="e3145-112">You can then create an Integration Map file that lets you visually map items between two XML schema files.</span></span> <span data-ttu-id="e3145-113">Ez a projekt létrehozása után fog az XSLT-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="e3145-113">After you build this project, you will have an XSLT document.</span></span>

## <a name="how-do-i-add-a-map"></a><span data-ttu-id="e3145-114">Hogyan vegyen fel egy társítást?</span><span class="sxs-lookup"><span data-stu-id="e3145-114">How do I add a map?</span></span>

1. <span data-ttu-id="e3145-115">Válassza ki az Azure-portálon **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="e3145-115">In the Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="e3145-116">A szűrő a keresőmezőbe írja be **integrációs**, majd jelölje be **integrációs fiókok** eredmények listájáról.</span><span class="sxs-lookup"><span data-stu-id="e3145-116">In the filter search box, enter **integration**, then select **Integration Accounts** from the results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="e3145-117">Válassza ki a integrációs fiókot, amelyik számára a társítás hozzáadása kívánja.</span><span class="sxs-lookup"><span data-stu-id="e3145-117">Select the integration account where you want to add the map.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="e3145-118">Válassza ki a **Maps** csempére.</span><span class="sxs-lookup"><span data-stu-id="e3145-118">Select the **Maps** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. <span data-ttu-id="e3145-119">Miután a Maps panel nyílik meg, válassza ki azt **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e3145-119">After the Maps blade opens, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. <span data-ttu-id="e3145-120">Adjon meg egy **neve** a térképen.</span><span class="sxs-lookup"><span data-stu-id="e3145-120">Enter a **Name** for your map.</span></span> <span data-ttu-id="e3145-121">A térkép fájl feltöltéséhez válassza ki a mappára a jobb oldalon a **térkép** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="e3145-121">To upload the map file, choose the folder icon on the right side of the **Map** text box.</span></span> <span data-ttu-id="e3145-122">A feltöltési folyamat befejezése után válassza ki a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3145-122">After the upload process completes, choose **OK**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. <span data-ttu-id="e3145-123">Miután az Azure ad hozzá a térkép integrációs fiókját, bemutatja, hogy a leképezés vagy nem lett-e adva képernyőn üzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="e3145-123">After Azure adds the map to your integration account, you get an onscreen message that shows whether your map file was added or not.</span></span> <span data-ttu-id="e3145-124">Miután ezt az üzenetet kap, válassza ki azt a **Maps** szeretné megjeleníteni az újonnan hozzáadott térkép csempére.</span><span class="sxs-lookup"><span data-stu-id="e3145-124">After you get this message, choose the **Maps** tile so you can view the newly added map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a><span data-ttu-id="e3145-125">Hogyan szerkeszthetők a térkép?</span><span class="sxs-lookup"><span data-stu-id="e3145-125">How do I edit a map?</span></span>

<span data-ttu-id="e3145-126">Akkor kell új térkép rendelkező fájlt tölthet fel a kívánt módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e3145-126">You must upload a new map file with the changes that you want.</span></span> <span data-ttu-id="e3145-127">A térkép szerkesztéséhez először töltheti le.</span><span class="sxs-lookup"><span data-stu-id="e3145-127">You can first download the map for editing.</span></span>

<span data-ttu-id="e3145-128">Töltse fel, amely lecseréli a meglévő térkép új leképezés, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="e3145-128">To upload a new map that replaces the existing map, follow these steps.</span></span>

1. <span data-ttu-id="e3145-129">Válassza ki a **Maps** csempére.</span><span class="sxs-lookup"><span data-stu-id="e3145-129">Choose the **Maps** tile.</span></span>

2. <span data-ttu-id="e3145-130">Után megnyílik a Maps panel, válassza ki a szerkeszteni kívánt leképezés.</span><span class="sxs-lookup"><span data-stu-id="e3145-130">After the Maps blade opens, select the map that you want to edit.</span></span>

3. <span data-ttu-id="e3145-131">Az a **Maps** paneljén válassza **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="e3145-131">On the **Maps** blade, choose **Update**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. <span data-ttu-id="e3145-132">Válassza ki a térkép fájlt, amelyet szeretne feltölteni, majd válassza ki a fájlválasztó **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="e3145-132">In the file picker, select the map file that you want to upload, then select **Open**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a><span data-ttu-id="e3145-133">Hogyan térképre törli?</span><span class="sxs-lookup"><span data-stu-id="e3145-133">How to delete a map?</span></span>

1. <span data-ttu-id="e3145-134">Válassza ki a **Maps** csempére.</span><span class="sxs-lookup"><span data-stu-id="e3145-134">Choose the **Maps** tile.</span></span>

2. <span data-ttu-id="e3145-135">Után megnyílik a Maps panel, válassza ki a törölni kívánt leképezés.</span><span class="sxs-lookup"><span data-stu-id="e3145-135">After the Maps blade opens, select the map you want to delete.</span></span>

3. <span data-ttu-id="e3145-136">Válasszon **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e3145-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. <span data-ttu-id="e3145-137">Győződjön meg arról, hogy szeretné-e a térkép törlése.</span><span class="sxs-lookup"><span data-stu-id="e3145-137">Confirm that you want to delete the map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a><span data-ttu-id="e3145-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e3145-138">Next Steps</span></span>
* [<span data-ttu-id="e3145-139">További tudnivalók a vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="e3145-139">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")  
* [<span data-ttu-id="e3145-140">További információ a megállapodások</span><span class="sxs-lookup"><span data-stu-id="e3145-140">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  
* [<span data-ttu-id="e3145-141">További tudnivalók átalakítások</span><span class="sxs-lookup"><span data-stu-id="e3145-141">Learn more about transforms</span></span>](logic-apps-enterprise-integration-transform.md "vállalati integrációs átalakítások ismertetése")  

